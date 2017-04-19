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

## Workshop #1 tasks:

1. Login to CF, and deploy `node` and `spring-music` example apps to CF.
    Example apps can be found in `./workshop-material/demo-apps`
2. Open these apps in browser to see if they work

---

## Workshop #1 questions:

1. How does CF support multiple languages? Can you extend it to support more languages/custom runtimes? (Reference: https://docs.cloudfoundry.org/buildpacks/)
2. Can you push your java source code directly to CF, and let CF build & compile & run it?
3. Run `cf target` from your terminal, and explains what each item(`API endpoint`, `User`, `Org`, `Space`) means.

---

## Workshop #1 cleanup:

When you're done with this workshop you can run the following command for all your apps to save some money (PWS price is calculated on a [per hour basis](https://run.pivotal.io/pricing/)):

`$ cf delete -r YOUR_APP_NAME`

You can also run `cf apps` to see all your apps in your targeted space.

---

# Workshop #2: Manifest & Logging

---

## Workshop #2 tasks:

1. Now you're familiar with `cf push` to deploy apps, you're required to deploy `articulate` application in the `demo-apps` directory, give it `512MB` memory and `random-route`. 
2. Observe `articulate` application's `logs` and `events` using CLI.
3. Don't delete the app yet as we need it for next workshop.

---

## Workshop #2 questions:

1. Where should your application write logs? Hint: see [here](https://12factor.net/logs)
2. What are some of the different origin codes seen in the log? (e.g. `API`, `STG`, `CELL`, `APP`, `RTR`). Please explain what each code means. For reference see [here](https://docs.cloudfoundry.org/devguide/deploy-apps/streaming-logs.html)
3. How does this change how you access logs today? At scale?

---

# Workshop #3: Scaling & High Availablity

---

## Workshop #3 tasks:

1.