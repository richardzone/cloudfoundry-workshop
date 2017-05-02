<!-- page_number: true -->
# Workshop cheatsheets

---

## Workshop #1 reference steps (20 mins):

1. Login from CLI: `cf login -a api.run.pivotal.io`
2. `cd workshop-material/demo-apps/node`
3. `cf push node-sample-app --random-route -m 128M`
4. open the app's URL in browser and see if it works
5. repeat the same steps for `python`/`spring-music` in `demo-apps` folder
6. Don't forget to delete the apps when you're done to reduce cost

---

## Workshop #2 reference steps (10 mins):

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

## Workshop #2 main steps (cont.) (5 mins):

1. Open your browser and view the application. Observe the log output, then refresh the page and you can see more logs are added.
2. Stop tailing logs by pressing `Control+C`
3. Run `cf events articulate` to see CF events for audit purposes.
4. Don't delete the app yet as we need it for next workshop.

---

## Workshop #3 main steps for Vertically Scale (10 mins):

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

## Workshop #3 main steps for Horizontally Scale (10 mins):

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

## Workshop #3 main steps for HA (5 mins):

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

---

## Workshop #4 main steps for managed service (15 mins):

1. 
    ```sh
    $ cd workshop-material/demo-apps/attendee-service
    $ cf push attendee-service -p ./attendee-service-0.0.1-SNAPSHOT.jar -m 512M --random-route
    ```
2. 
    ```sh
    $ cf marketplace
    $ cf create-service cleardb spark attendee-mysql
    $ cf bind-service attendee-service attendee-mysql
    $ cf restart attendee-service
    ```
3. View the `attendee-service` in a browser.

---

## Workshop #4 main steps for user provided service instance (10 mins):

1. `articulate`'s default configuration for the `attendee-service` `uri` is http://localhost:8181/attendees. The subsequent steps will allow you to override the default configuration with your own.
2. `cf create-user-provided-service attendee-service -p uri`
    **Note**: The CLI will ask you interactively for "uri" parameter, you should answer like "https://attendee-service-XXX-XXX.cfapps.io/attendees"
3. Execute the following commands:
    ```sh
    $ cf bind-service articulate attendee-service
    $ cf restart articulate
    $ cf env articulate  # To review the environment
    ```
4. Navigate to `articulate`'s "Services" page and try to add some attendees.

---

## Workshop #5 main steps (20 mins):

1. Browse to the `articulate` `Blue-Green` page. And generate some requests by pressing the `Start` button. Don't close this page yet.
    Observe our current version (v1) handle all web requests.
2. Run `cf routes` and take note of the current `host` of `articulate`.
3. `cf push articulate-v2 -p ./articulate-0.0.1-SNAPSHOT.jar -m 512M -n articulate-prodtest`
4. `cf bind-service articulate-v2 attendee-service`
5. `cf restart articulate-v2`
6. Open a new tab and view version 2 of `articulate` in your browser. Take note of the application name. You can do further testing on this environment.

---

## Workshop #5 main steps (cont.):

7. When we're ready to release this version, run `$ cf map-route articulate-v2 cfapps.io -n articulate-XXX` where `articulate-XXX` is version 1's hostname.
8. Return to the original browser tab of Blue-Green` page where you're generating requests. You should see that it is starting to send requests to version 2.
9. Press the `Reset` button on the page, so we can see how the load get distributed across app instances. Run `cf apps` to see how many instances of each app version.

---

## Workshop #5 main steps (cont.):

10. Move more traffic to version 2.
    ```sh
    $ cf scale articulate -i 1
    $ cf scale articulate-v2 -i 2
    ```
11. Move all traffic to version 2 by running `$ cf unmap-route articulate cfapps.io -n articulate-version1-route`
    If you `Reset` the requests generator, you will see all the traffic goes to `articulate-v2`.
12. Remove the temp route from the articulate-v2 application: `$ cf unmap-route articulate-v2 cfapps.io -n articulate-prodtest`
13. Clean up:
    ```sh
    $ cf delete articulate
    $ cf rename articulate-v2 articulate
    $ cf restart articulate
    $ cf scale articulate -i 1
    ```