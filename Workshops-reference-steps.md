<!-- page_number: true -->
# Workshop cheatsheets

---

## Workshop #1 reference steps:

1. Login from CLI: `cf login -a api.run.pivotal.io`
2. `cd workshop-material/demo-apps/node`
3. `cf push node-sample-app --random-route -m 128M`
4. open the app's URL in browser and see if it works
5. repeat the same steps for `python`/`spring-music` in `demo-apps` folder
6. Don't forget to delete the apps when you're done to reduce cost

---

## Workshop #2 reference steps:

1. `cd workshop-material/demo-apps/articulate`
2. Write a [manifest](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html) file `manifest.yml` with the following content, be aware that **indention matters** with [YAML syntax](http://docs.ansible.com/ansible/YAMLSyntax.html) :
    ```yaml
    ---
    applications:
    - name: articulate
      memory: 512M
      random-route: true
      path: ./articulate-0.0.1-SNAPSHOT.jar
    ```
3. `cf push --no-start` to push your app (it will automatically find `manifest.yml` in your working directory and use it as configuration)
4. `cf logs articulate` to tail the logs of the `articulate` app
5. Open another terminal and run `cf start articulate` to start the app.

---

## Workshop #2 main steps (cont.):

1. Open your browser and view the application. Observe the log output, then refresh the page and you can see more logs are added.
2. Stop tailing logs by pressing `Control+C`
3. Run `cf events articulate` to see CF events for audit purposes.
4. Don't delete the app yet as we need it for next workshop.

---

## Workshop #3 main steps for Vertically Scale:

1. Start tailing the `articulate` app log again:
    ```sh
    $ cf logs articulate | grep "API\|CELL"
    ```
    `grep ` filters log lines from the [Cloud Controller](https://docs.cloudfoundry.org/concepts/architecture/cloud-controller.html) and [Cell](https://docs.cloudfoundry.org/concepts/architecture/#diego-cell) components.
2. Vertically scale the app (scale up): Open another terminal and run:
    ```sh
    cf scale articulate -m 1G
    ```
3. Observe log output.
4. Scale `articulate` back to original settings.
    ```sh
    $ cf scale articulate -m 512M
    ```
    
---

## Workshop #3 main steps for Horizontally Scale:

1. Browse to the `Scale and HA` page of the `articulate` application. Review the `Application Environment Information`.
2. Press the `Refresh` button multiple times. All requests are going to one application instance.
3. Start tailing the logs: 
    ```sh
    $ cf logs articulate | grep 'API\|CELL'
    ```
4. Open another terminal and scale the application to 3 instances:
    ```sh
    cf scale articulate -i 3
    ```
5. Observe log output. Then stop tailing the logs.
6. Return to `articulate` in a web browser. Press the `Refresh` button several times. Observe the `Addresses` and `Instance Index` changing.

---

## Workshop #3 main steps for HA:

1. At this time you should be running multiple instances of `articulate`. Confirm this with the following command:
    ```
    $ cf app articulate
    ```
2. Return to `articulate` in browser and navigate to `Scale and HA` page. Press the `Refresh` button. Confirm the application is running.
3. Kill the app. Press the `Kill` button!
4. `cf app articulate` to check app state.
5. `Refresh` the page to see if app is back up.
6. `cf events articulate` to see which app instance was killed.
7. `cf scale articulate -i 1` to scale app back to original settings.
