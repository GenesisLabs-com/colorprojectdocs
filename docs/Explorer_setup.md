# Color Explorer
Block Explorer for Color

## How to run The Color Explorer
1. Clone the color explorer github repo. <br />
    ```https://github.com/UsmanFazil/explorer```
2. Copy `default_settings.json` to `settings.json`.
3. Update chain-id.
4. Update the RPC URL.
5. Update the LCD URL.
6. Update Bech32 address prefixes.
7. Update genesis file location.

### Run in local

```
meteor npm install
meteor update
meteor --settings settings.json
```

### Run in production

```
./build.sh
```

It will create a packaged Node JS tarball at `../output`. Deploy that packaged Node JS project with process manager like [forever](https://www.npmjs.com/package/forever) or [Phusion Passenger](https://www.phusionpassenger.com/library/walkthroughs/basics/nodejs/fundamental_concepts.html).

