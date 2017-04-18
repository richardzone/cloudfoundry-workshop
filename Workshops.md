<!-- page_number: true -->
# Preparation

1. Sign up free account for Pivotal Web Services: http://run.pivotal.io/
2. Install Cloud Foundry CLI: http://docs.pivotal.io/pivotalcf/1-10/cf-cli/install-go-cli.html
3. Push a sample Java app to test the configuration:
    ```sh
    $ cf login -a api.run.pivotal.io   # Please select Development space
    $ git clone https://github.com/cloudfoundry-samples/spring-music
    $ cd spring_music/ ; ./gradlew assemble
    $ cf push
    ```
    Look for `urls: spring-music-XXX-XXX.cfapps.io` in `cf push` command output, then open that URL in your browser.
4. Delete the app to save your money:
    ```
    cf delete -fr spring-music
    ```
5. All done!


---

# PCF workshop reference material

- Open source CF docs can be found at https://docs.cloudfoundry.org/
- If you need to lookup Pivotal's PCF proprietary stuff: http://docs.pivotal.io/pivotalcf/
- Docs for Pivotal's public cloud - Pivotal Web Services (PWS) can be found at: https://docs.run.pivotal.io/
- 12 factor cloud native apps: https://12factor.net/

---

# Workshop #1: From source code to PaaS cloud

---

## Workshop #1 main steps:

1. Login from CLI: `cf login -a api.run.pivotal.io`
2. `cd workshop-material/demo-apps/node`
3. `cf push node-sample-app --random-route -m 128M`
4. open the app's URL in browser and see if it works
5. repeat the same steps for `python`/`spring-music` in `demo-apps` folder

---

## Workshop #1 questions:

1. How does CF support multiple languages? Can you extend it to support more languages/custom runtimes? (Reference: https://docs.cloudfoundry.org/buildpacks/)
2. Run `cf target` from your terminal, and explains what each item(`API endpoint`, `User`, `Org`, `Space`) means.

---

## Workshop #1 cleanup:

When you're done with this workshop you can run the following command for all your apps to save some money (PWS price is calculated on a [per hour basis](https://run.pivotal.io/pricing/)):

`$ cf delete -r YOUR_APP_NAME`

You can also run `cf apps` to see all your apps in your targeted space.

---

# Workshop #3: From source code to PaaS cloud

---