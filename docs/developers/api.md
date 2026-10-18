# Extending the Haveno API

This guide is for contributors who add or change calls in Haveno's API. It covers how the API is built, how to run its end-to-end tests, and how to add a new call.

!!! info "Just want to use the API?"
    To build a program on top of Haveno, see [Haveno API](../users/haveno-api.md). The calls are documented in the [API reference](https://haveno-dex.github.io/haveno-ts/typedocs/classes/HavenoClient.HavenoClient.html).

## How it works

The API spans two repositories: [haveno](https://github.com/haveno-dex/haveno), which serves it from the daemon, and [haveno-ts](https://github.com/haveno-dex/haveno-ts), the TypeScript client and its end-to-end tests. A call passes through these layers:

```
HavenoClient (haveno-ts)  →  Envoy  →  Grpc*Service  →  CoreApi  →  Core*Service
```

| Layer | Location | Role |
| ----- | -------- | ---- |
| Protobuf definitions | [`proto/.../grpc.proto`](https://github.com/haveno-dex/haveno/blob/master/proto/src/main/proto/grpc.proto) | Services, requests, and replies |
| gRPC services | [`daemon/.../grpc/`](https://github.com/haveno-dex/haveno/tree/master/daemon/src/main/java/haveno/daemon/grpc) | Convert between protobuf and core types, handle errors, set rate limits |
| Core API | [`core/.../api/CoreApi.java`](https://github.com/haveno-dex/haveno/blob/master/core/src/main/java/haveno/core/api/CoreApi.java) | Single entry point to the core |
| Core services | [`core/.../api/`](https://github.com/haveno-dex/haveno/tree/master/core/src/main/java/haveno/core/api) | Implement each call against wallets, offers, trades, and the network |
| Client | [`src/HavenoClient.ts`](https://github.com/haveno-dex/haveno-ts/blob/master/src/HavenoClient.ts) | Typed method for each call |
| Tests | [`src/HavenoClient.test.ts`](https://github.com/haveno-dex/haveno-ts/blob/master/src/HavenoClient.test.ts) | End-to-end tests against live daemons |

Envoy translates the browser-compatible gRPC-Web used by haveno-ts into gRPC. The daemon checks the API password and rate limits before a call reaches its service.

For example, `getOffers` is served by [`GrpcOffersService.getOffers`](https://github.com/haveno-dex/haveno/blob/master/daemon/src/main/java/haveno/daemon/grpc/GrpcOffersService.java), which calls [`CoreApi.getOffers`](https://github.com/haveno-dex/haveno/blob/master/core/src/main/java/haveno/core/api/CoreApi.java), then [`CoreOffersService.getOffers`](https://github.com/haveno-dex/haveno/blob/master/core/src/main/java/haveno/core/api/CoreOffersService.java).

## Set up a test network

The tests run against a local Monero testnet and Haveno network on your machine.

### Requirements

- [Haveno build dependencies](installing.md#install-dependencies)
- [Node.js](https://nodejs.org)
- The [Envoy](https://www.envoyproxy.io/docs/envoy/latest/start/install.html) proxy, e.g. `brew install envoy`
- The protobuf compiler [`protoc` v29.3](https://github.com/protocolbuffers/protobuf/releases/tag/v29.3), with the `protoc-gen-js` and `protoc-gen-grpc-web` plugins [installed as shown here](https://github.com/grpc/grpc-web#code-generator-plugins)

### Start the network

1. Clone both repositories into the same parent directory, since haveno-ts builds against `../haveno`, and build Haveno:

    ```bash
    git clone https://github.com/haveno-dex/haveno.git
    git clone https://github.com/haveno-dex/haveno-ts.git
    cd haveno
    make skip-tests
    ```

2. From `haveno`, start each of the following in its own terminal:

    ```bash
    make monerod1-local       # Monero node
    make monerod2-local       # second Monero node
    make seednode-local       # Haveno seed node
    make funding-wallet-local # wallet that funds the tests
    ```

3. From `haveno-ts`, start Envoy in another terminal:

    ```bash
    envoy -c config/envoy.local.yaml
    ```

4. From `haveno-ts`, install dependencies. This also generates the client's protobuf code from `../haveno/proto`:

    ```bash
    npm install
    ```

The tests start the arbitrator, user1, and user2 daemons if they are not already running, and handle mining, funding, and arbitrator registration. To follow a daemon's output, or to debug it, start it yourself first with `make arbitrator-daemon-local`, `make user1-daemon-local`, or `make user2-daemon-local`. The tests connect to running daemons instead of starting new ones.

??? note "Windows"
    Run the commands from Git Bash, or MSYS2 MINGW64 for the `make` targets. Envoy runs in WSL2, or download [Envoy v1.28.0](https://archive.tetratelabs.io/envoy/download/v1.28.0/envoy-v1.28.0-windows-amd64.tar.xz) and add it to your `PATH`. Install the JavaScript plugin with `npm install --global protoc-gen-js`. See the [haveno-ts test notes](https://github.com/haveno-dex/haveno-ts/blob/master/docs/run-api-tests.md#notes-for-windows) for details.

??? note "Docker"
    To work on haveno-ts alone, start the whole network with `docker compose up` from `haveno-ts/docker` instead. It builds Haveno's `master` branch, so it does not include your local changes to Haveno.

## Run the tests

From `haveno-ts`, run a test by name:

```bash
npm run test -- --baseCurrencyNetwork=XMR_LOCAL -t "Can get the version"
```

Omit `-t` to run the full suite. To run the subset that runs in continuous integration, select the tests tagged `CI`:

```bash
npm run test -- --baseCurrencyNetwork=XMR_LOCAL -t "(?:^|\W)CI(?:$|\W)"
```

The daemons log errors as the tests exercise invalid inputs, so errors in their output are expected.

!!! tip
    Daemons started by the tests run the current build of Haveno. A daemon you started yourself keeps running the old code, so restart it after rebuilding.

## Add an API call

1. **Define the call.** Add the `rpc` and its request and reply messages to [`grpc.proto`](https://github.com/haveno-dex/haveno/blob/master/proto/src/main/proto/grpc.proto). Give new fields new numbers, and never renumber or reuse an existing field, so existing clients keep working.

2. **Implement it in the core.** Add the logic to the matching `Core*Service` in [`core/.../api/`](https://github.com/haveno-dex/haveno/tree/master/core/src/main/java/haveno/core/api), and expose it through [`CoreApi`](https://github.com/haveno-dex/haveno/blob/master/core/src/main/java/haveno/core/api/CoreApi.java).

3. **Serve it from the daemon.** Override the generated method in the matching `Grpc*Service`, following its existing methods:

    ```java
    @Override
    public void getOffers(GetOffersRequest req, StreamObserver<GetOffersReply> responseObserver) {
        try {
            List<OfferInfo> result = coreApi.getOffers(req.getDirection(), req.getCurrencyCode())
                    .stream().map(OfferInfo::toOfferInfo)
                    .collect(Collectors.toList());
            var reply = GetOffersReply.newBuilder()
                    .addAllOffers(result.stream().map(OfferInfo::toProtoMessage).collect(Collectors.toList()))
                    .build();
            responseObserver.onNext(reply);
            responseObserver.onCompleted();
        } catch (Throwable cause) {
            exceptionHandler.handleException(log, cause, responseObserver);
        }
    }
    ```

    Then add a rate limit for the call in the service's `rateMeteringInterceptor()`. A call without one is not rate limited.

4. **Rebuild Haveno** from `haveno`:

    ```bash
    make clean && make skip-tests
    ```

5. **Regenerate the client's protobuf code** from `haveno-ts`:

    ```bash
    npm install
    ```

6. **Add a client method** to [`HavenoClient.ts`](https://github.com/haveno-dex/haveno-ts/blob/master/src/HavenoClient.ts). Its doc comment becomes the [API reference](https://haveno-dex.github.io/haveno-ts/typedocs/classes/HavenoClient.HavenoClient.html), so describe each parameter and the return value:

    ```ts
    /**
     * Get available offers to buy or sell XMR.
     *
     * @param {string} assetCode - traded asset code
     * @param {OfferDirection|undefined} direction - "buy" or "sell" (default all)
     * @return {OfferInfo[]} the available offers
     */
    async getOffers(assetCode: string, direction?: OfferDirection): Promise<OfferInfo[]> {
      try {
        ...
      } catch (e: any) {
        throw new HavenoError(e.message, e.code);
      }
    }
    ```

7. **Add tests** to [`HavenoClient.test.ts`](https://github.com/haveno-dex/haveno-ts/blob/master/src/HavenoClient.test.ts), following the existing tests. Include `CI` in a test's name tags, e.g. `"Can get offers (Test, CI)"`, to run it in continuous integration.

8. **Run the tests**, first your new test by name and then the full suite, as in [Run the tests](#run-the-tests).

9. **Optionally add it to the CLI.** [Haveno CLI](../users/haveno-cli.md) commands are defined in [`cli/`](https://github.com/haveno-dex/haveno/tree/master/cli/src/main/java/haveno/cli), in `Method.java`, `GrpcClient.java`, and `CliMain.java`.

10. **Open pull requests** to [haveno](https://github.com/haveno-dex/haveno) and [haveno-ts](https://github.com/haveno-dex/haveno-ts). The haveno-ts tests build Haveno's `master` branch, so they pass once the haveno pull request is merged.

## Further reading

- [**API reference**](https://haveno-dex.github.io/haveno-ts/typedocs/classes/HavenoClient.HavenoClient.html): complete method documentation
- [**Haveno API**](../users/haveno-api.md): using the API from a program
- [**Build and run Haveno**](installing.md): building Haveno and running a local network by hand
- [**Contributor guidelines**](contributor-guidelines.md): how to submit changes

!!! info "Questions?"
    For help, join the [Matrix room](https://matrix.to/#/#haveno:monero.social).
