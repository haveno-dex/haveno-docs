# haveno-ts Library

haveno-ts is a library users and programs can use to interact with Haveno without necessarily needing a user interface. It's a set of API calls that can be integrated on any platform, wallet or application:

[:material-github: haveno-ts](https://github.com/haveno-dex/haveno-ts){ .md-button .md-button--primary }

The [new user interface](haveno-ui.md) will use haveno-ts to communicate with the Haveno core.

If you are interested in integrating Haveno into your app, let us know and we'll be happy to help you out.

## Documented API calls

Documentation is automatically generate using Typedoc: 

[:material-web: API Docs](https://haveno-dex.github.io/haveno-ts/classes/HavenoClient.HavenoClient.html){ .md-button .md-button--primary }


## Run API tests

Running the [API tests](https://github.com/haveno-dex/haveno-ts/blob/master/src/HavenoClient.test.ts) is the best way to develop and test Haveno end-to-end.

[`HavenoClient.ts`](https://github.com/haveno-dex/haveno-ts/blob/master/src/HavenoClient.ts) provides the client interface to Haveno's backend daemon.

1. [Run a local Haveno test network](https://github.com/haveno-dex/haveno/blob/master/docs/installing.md) and then shut down the arbitrator, Alice, and Bob or run them as daemons, e.g. `make alice-daemon`. You may omit the arbitrator registration steps since it is done automatically in the tests.
2. Clone this project to the same parent directory as the haveno project: `git clone https://github.com/haveno-dex/haveno-ts`
3. In a new terminal, start envoy with the config in haveno-ts/config/envoy.test.yaml (change absolute path for your system): `docker run --rm --add-host host.docker.internal:host-gateway -it -v ~/git/haveno-ts/config/envoy.test.yaml:/envoy.test.yaml -p 8079:8079 -p 8080:8080 -p 8081:8081 -p 8082:8082 -p 8083:8083 -p 8084:8084 -p 8085:8085 -p 8086:8086 envoyproxy/envoy-dev:8a2143613d43d17d1eb35a24b4a4a4c432215606 -c /envoy.test.yaml`
4. In a new terminal, start the funding wallet. This wallet will be automatically funded in order to fund Alice and Bob during the tests.<br>For example: `cd ~/git/haveno && make funding-wallet`.
5. Install protobuf compiler v3.19.1 or later for your system:<br>
    mac: `brew install protobuf`<br>
    linux: `apt install protobuf-compiler`
    NOTE: You may need to upgrade to v3.19.1 manually if your package manager installs an older version.
6. Download `protoc-gen-grpc-web` plugin and make executable as [shown here](https://github.com/grpc/grpc-web#code-generator-plugin).
7. `cd haveno-ts`
8. `npm install`
9. `npm test` to run all tests or `npm run test -- -t 'my test'` to run tests by name.

## Adding new API functions and tests

1. Follow [instructions](https://github.com/haveno-dex/haveno-ts#run-tests) to run Haveno's existing API tests successfully.
2. Define the new service or message in Haveno's [protobuf definition](https://github.com/haveno-dex/haveno/blob/master/proto/src/main/proto/grpc.proto).
3. Clean and build Haveno after modifying the protobuf definition: `make clean && make`
4. Implement the new service in Haveno's backend, following existing patterns.<br>
   For example, the gRPC function to get offers is implemented by [`GrpcServer`](https://github.com/haveno-dex/haveno/blob/master/daemon/src/main/java/bisq/daemon/grpc/GrpcServer.java) > [`GrpcOffersService.getOffers(...)`](https://github.com/haveno-dex/haveno/blob/b761dbfd378faf49d95090c126318b419af7926b/daemon/src/main/java/bisq/daemon/grpc/GrpcOffersService.java#L104) > [`CoreApi.getOffers(...)`](https://github.com/haveno-dex/haveno/blob/b761dbfd378faf49d95090c126318b419af7926b/core/src/main/java/bisq/core/api/CoreApi.java#L128) > [`CoreOffersService.getOffers(...)`](https://github.com/haveno-dex/haveno/blob/b761dbfd378faf49d95090c126318b419af7926b/core/src/main/java/bisq/core/api/CoreOffersService.java#L126) > [`OfferBookService.getOffers()`](https://github.com/haveno-dex/haveno/blob/b761dbfd378faf49d95090c126318b419af7926b/core/src/main/java/bisq/core/offer/OfferBookService.java#L193).
5. Build Haveno: `make`
6. Update the gRPC client in haveno-ts: `npm install`
7. Add the corresponding typescript method(s) to [HavenoClient.ts](https://github.com/haveno-dex/haveno-ts/blob/master/src/HavenoClient.ts) with clear and concise documentation.
8. Add clean and comprehensive tests to [HavenoClient.test.ts](https://github.com/haveno-dex/haveno-ts/blob/master/src/HavenoClient.test.ts), following existing patterns.
9. Run the tests with `npm run test -- -t 'my test'` to run tests by name and `npm test` to run all tests together. Ensure all tests pass and there are no exception stacktraces in the terminals of Alice, Bob, or the arbitrator.
10. Open pull requests to the haveno and haveno-ts projects for the backend and frontend implementations.
