# Project Phoenix Introduction

Project Phoenix is a new design system that is intended to close the gap between our designers, UI developers, AEM developers, and product teams. Currently all of our components are built in Angular and showcased on the styleguide site (Angular), but they must be consumed by our AEM devs within a very different tech stack. This can make for a painful process, and also allows walls to be built between teams that need to be collaborating.

With Project Phoenix, we want to close the gap between UI devs and AEM devs. By building our component library as AEM components themselves, we can break down some barriers, and have a more common language and understanding to solve problems for our users together.

If you haven't seen the fantastic presentation Vivian Huang put together on [Design Systems and Frameworks](https://tmobileusa.sharepoint.com/:p:/s/GELProject/ETEUbRloU51GiBlZrdXNaO0B-pCGOX-_r88AyMhjafr8eA?e=xUMv7F), I encourage you to go check it out to get familiar with our design system process at T-Mobile.

In this document we'll be diving into this step of the process -

![Screen Shot 2022-01-12 at 12 44 05 PM](https://user-images.githubusercontent.com/13723156/149202493-44a20494-bfbb-49f5-856b-6ad5c15382a0.png)

> _This is where we develop components in AEM, update the new style guide, and release the component package for our AEM dev partners to start using._

## Setting up your machine for Phoenix AEM development

- If you don't have Java installed on your machine, you'll want to install it locally so you can run AEM. As of 1/12/22 Version 11 is the correct one. You can grab it [here](https://www.oracle.com/java/technologies/javase/jdk11-archive-downloads.html)

  [![Java 11 download](https://user-images.githubusercontent.com/13723156/149221220-5feaa08e-559d-4a10-9a3a-9f028ce78c35.png)](https://www.oracle.com/java/technologies/javase/jdk11-archive-downloads.html)

  - Create an Oracle acct, and download
  - Install the JDK
  - Check Java version

  ```sh
  java -version
  ```

  ![java_version_check](https://user-images.githubusercontent.com/13723156/149221645-2fdb1efb-31d7-404a-a2c6-958863e42505.gif)

  - **NOTE**: You may need to set your `JAVA_HOME` environment variable to point directly to where you installed version 11 of Java. Follow these steps if you're having issues with your Java installation:

    1. First, find your actual Java SDK Home directory by pasting this directy in your terminal: (will output your java installation location)

       ```sh
       /usr/libexec/java_home
       ```

    2. Manually navigate there to make sure you don't have any mistakes due to incorrect versions, etc. For me, this was:

       ```sh
       /Library/Java/JavaVirtualMachines/jdk-13.0.2.jdk/Contents/Home
       ```

    3. Next, edit your terminal's profile. If you're using zsh and VSCode, this will be:

       ```sh
       code ~/.zshrc
       ```

    4. Inside, add the following new line anywhere in the file:

       ```sh
       export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-13.0.2.jdk/Contents/Home
       ```

    5. Restart your terminal app (or run `source ~/.zshrc`), and it should work properly.

       _steps taken from [this stackoverflow question](https://stackoverflow.com/questions/22842743/how-to-set-java-home-environment-variable-on-mac-os-x-10-9)_

- You'll also need [Docker](https://desktop.docker.com/mac/main/amd64/Docker.dmg) installed

  - Make sure to adjust the allocated resources for your containers so that you don't get timeout issues (easiest to change in Docker desktop app)

    ```sh
    CPUs:       6
    Memory:     6GB
    Swap:       1.5GB
    Disk image: 59.6GB
    ```

    ![docker_resource_settings](https://user-images.githubusercontent.com/13723156/149223110-6d46a613-6cde-4c08-bf05-954a165e14ff.png)

- Install Maven (Java package manager)

  - Maven requires Java, so make sure you finished downloading Java!
  - Download the Maven **binary** zip https://maven.apache.org/download.cgi
  - Extract the file, and navigate to it's location (for this example it is in `Downloads`)
  - Add the `bin` directory to your `PATH` variable

    ```sh
    cd ~/Downloads/apache-maven-x.x.x/bin # replace x.x.x with your version num
    ```

    ```sh
    pwd # copy output
    ```

    Add this to your `.zhsrc` or `.bash_profile`

    ```sh
    export PATH=COPIED_OUTPUT:$PATH
    ```

    ![add_mvn_to_path](https://user-images.githubusercontent.com/13723156/149223806-67b6b25e-13b4-4f37-b651-d83bf80d20d3.gif)

  - Close and re-open your terminal (or run `source ~/.zshrc`)
  - Verify installation was successful

    ```sh
    mvn -v
    ```

    ![mvn_check_version](https://user-images.githubusercontent.com/13723156/149224031-03a6a8a9-db3a-43c2-905c-117621666cd4.gif)

- Lastly you will need to generate a new GitLab Personal Access Token (PAT) to be able to access the correct `npm` packages

  - Follow the instructions on [this page](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html) to generate a new token. Make sure to check the boxes to give it full access.

    ![gitlab_pat](https://user-images.githubusercontent.com/13723156/149224531-578dba75-077b-4a85-90e0-9ceaf4512578.gif)

  - Add your PAT to an environment variable similar to how we set `JAVA_HOME`

    Add this to your `.zhsrc` file

    ```sh
    export GITLAB_PAT=YOUR_GITLAB_PAT
    ```

  - Close and re-open the terminal, then authenticate Docker with your PAT so your containers can access everything they need in our private GitLab repositories

    - Verify that your token has been saved as an environment variable.

      ```sh
      echo $GITLAB_PAT # should output token
      ```

    - Authenticate

      ```sh
      docker login -u YOUR_NTID -p ${GITLAB_PAT} registry.gitlab.com
      ```

## Using the Phoenix CLI

- The **platform team** has created a powerful CLI tool for us that makes our lives a lot easier. It contains two Docker images, one running nginx, and the other runs a containerized AEM instance.
- Head over to the [phoenix-cli](https://gitlab.com/tmobile/digital/tos/phoenix/phoenix-cli) repo and follow the **CLI installation** instructions. You can stop when you reach `Docker Images`.
- Read through those first steps carefully though, and make sure not to skim over anything.

  - If you hit any snags running the .exec file, try using right-click + 'open' to avoid the "unknown developer" issue.

    ![dl_cli_mac](https://user-images.githubusercontent.com/13723156/153077140-b6504c82-eccd-4f57-a78b-80c3c2998591.gif)

    ![open_cli](https://user-images.githubusercontent.com/13723156/153077164-dc48620d-dcdb-4fc4-907d-820c579334e4.gif)

  - Don't forget to add the directory to your `PATH`. If you keep the default installation path, it will look like this:

    ```sh
    export PATH="$PATH:/Users/YOUR_USERNAME/.phoenix-cli"
    ```

- After installation, restart your terminal and run `phoenix doctor`, and you should see a successful output. If you see any errors, you'll need to go back and make sure you completed all the previous steps successfully.

> _NOTE: Make sure you are on at **least** Docker Desktop version 4. `phoenix doctor` will show that Docker is not on your system if it is below version 4._

![phoenix doctor](https://user-images.githubusercontent.com/13723156/149347445-ec2593e5-ad3f-41b7-aafd-abb6fe651e16.png)

- For a refresher on available Phoenix CLI commands, run `phoenix help`
- The two most useful commands will be `raise` and `ashes` -

```
  raise              The raise command uses the docker-compose.yml file in the working directory to start the local
                     aem environment.
  ashes [options]    The ashes command uses the docker-compose.yml file in the working directory to stop the local aem
                     environment.
```

- Now let's run that `raise` command to actually spin up our AEM instance!

```sh
phoenix raise
```

> _If you are in a directory that contains a `docker-compose.yml` file when you run `phoenix raise`, it will use **that** file to start the local AEM env. Otherwise it will execute from the CLI installation location (which is what we're doing here)_

![phoenix raise start](https://user-images.githubusercontent.com/13723156/149353050-55f07eb6-52b4-46b5-9111-71422e25d868.gif)

After running `phoenix raise` you should be able to see two docker containers running, an `aem` one, and an `nginx` one.

![phoenix raise success](https://user-images.githubusercontent.com/13723156/149353398-b28c2dbb-63b0-4e0d-937a-015de4dde784.gif)

## Logging in and configuring your local AEM instance

- Now that our Docker images are running, we can access our AEM instance at `localhost:4502`. The login credentials for a local AEM instance are always

```
username: admin
password: admin
```

![aem login](https://user-images.githubusercontent.com/13723156/149366662-93d0cad6-fe31-4e61-828f-228cf1f267e3.gif)

- Next we need to get templates and content loaded into our AEM instance

  > The [`phoenix-platform`](https://gitlab.com/tmobile/digital/tos/phoenix/phoenix-platform) repository contains the templates, dependendencies, and a few other other libraries we'll need to get our AEM instance fleshed out.

  > The [`phoenix-experience`](https://gitlab.com/tmobile/digital/tos/phoenix/phoenix-experience) repository contains the content, components, and clientlibs (front-end CSS/JS) needed so that an AEM dev has something to work with.

- Before we install these packages, let's take a look at where the packages get installed. AEM provides a tool for viewing which packages are installed in the current AEM instance called **CRX Package Manager** (content repository extreme package manager).
- Navigate back to the AEM start page (click logo in top left of page)
- Click the hammer icon to get to tools, then select CRXDE Lite
- This should open up a very _Windows 98-esque_ UI. Click on the `package` icon in the top navbar

  ![arrow to package mgr](https://user-images.githubusercontent.com/13723156/149368120-80cc7248-847a-491d-8d7f-c9dc4889b49e.png)

- Most of the packages you see on this page are automatically installed. But the ones we want to focus in on are the ones that come directly from our phoenix repos. Click on the `com.tmobile.digital.tos.phoenix` group in the left hand navigation to narrow what's displayed.

  ![phoenix packages only](https://user-images.githubusercontent.com/13723156/149370318-fcc32624-c4e4-4e76-b6da-db71aa05bbfd.gif)

  > _You can think of this view as being similar to a `node_modules` folder that contains packages that are installed by a package manager_

- Let's start by installing the packages for `phoenix-platform`. Since AEM is built on Java, these packages need to be installed with Maven (which is kindof like NPM for Java). When we run the package installation command, it's going to find the running instance of AEM, and install the packages there. So we can watch them get installed if we keep our CRX Package manager page open!

  - If you haven't already, go ahead and clone down the [platform repository](https://gitlab.com/tmobile/digital/tos/phoenix/phoenix-platform)
  - Run this command in the phoenix-platform directory. The flag `-PautoInstallSinglePackage` bundles together a package that will be installed on the local instance of AEM. (Without that flag, no package will get installed)

    ```sh
    mvn clean install -PautoInstallSinglePackage
    ```

  ![mvn install platform](https://user-images.githubusercontent.com/13723156/149374067-cbfa0ff5-549d-48a6-8377-4ea6050f6a6e.gif)

  - Now when we reload the CRX Package Manager page, we should see the new `platform.all` zip file has been installed. This gives us critical templates for editing AEM pages.

  ![Screen Shot 2022-01-13 at 10 56 39 AM](https://user-images.githubusercontent.com/13723156/149374373-c5b8844e-112e-4ac7-bf1c-40b66f57259f.png)

- Next clone down the [experience repository](https://gitlab.com/tmobile/digital/tos/phoenix/phoenix-experience), and navigate into it's directory.

  - Go ahead and run the same command as before

    ```sh
    mvn clean install -PautoInstallSinglePackage
    ```

  ![mvn install experience](https://user-images.githubusercontent.com/13723156/149375744-99ac8586-87da-49ab-9995-9f435ec51275.gif)

  - You should see that we have one more package added to `com.tmobile.digital.tos.phoenix`!

## Adding in content

- Now that we have everything set up and configured correctly in our local AEM environment, the last step is to load in the content that our teammates have been working on! Bill Dinger or someone from the platform team will send out updated releases of new content, and all we need to do is download that .zip file that was shared, and then upload it as a package into the CRX Package Manager.

  ![Screen Shot 2022-01-13 at 11 21 11 AM](https://user-images.githubusercontent.com/13723156/149378563-196c4e14-0d74-46c9-9c64-42e16acd0b25.png)

  ![install content one](https://user-images.githubusercontent.com/13723156/149378823-6ed83879-0f02-488b-ae55-e544848014c3.gif)

  ![phoenix content done](https://user-images.githubusercontent.com/13723156/149378773-1ef8f639-3df3-4901-891f-4868836fdfe0.gif)

- Once we've done that, we'll then be able to see content and components show up when we head into the page editor

  ![finally done](https://user-images.githubusercontent.com/13723156/149380052-746f80c4-557e-472d-941a-2ecbaeb608a5.gif)

In the [next guide](./PHOENIX_DS.md) we'll take a look at where the styles come from for these components, and how you can update them!

## Troubleshooting

### Failed to load data

![image](https://i.imgur.com/X4dNupE.png)

If you run into an error that says `fail to load data`, these steps should help get you back on track.

1. Use the phoenix cli to kill, clean, and then restart your docker images

   ```sh
   phoenix ashes --purge
   ```

   ```sh
   phoenix raise
   ```

2. you may need to run the maven installation command on a couple other repositories ([analytics](https://gitlab.com/tmobile/digital/tos/phoenix/phoenix-analytics) and [unav](https://gitlab.com/tmobile/digital/tos/phoenix/phoenix-unav)). Clone these both down, and run `mvn clean install -PautoInstallSinglePackage` in both to install their packages.

   ```sh
    git clone git@gitlab.com:tmobile/digital/tos/phoenix/phoenix-unav.git

    cd phoenix-unav

    mvn clean install -PautoInstallSinglePackage
   ```

   ```sh
    git clone git@gitlab.com:tmobile/digital/tos/phoenix/phoenix-analytics.git

    cd phoenix-analytics

    mvn clean install -PautoInstallSinglePackage
   ```

3. Hard refresh your local AEM instance, and you should now be able to access test pages. If you're still having issues, here is [another guide](https://confluencesw.t-mobile.com/pages/viewpage.action?pageId=231318200) for resolving this issue.

### No Components in AEM Local Env

- Paige Libbert put together a [great guide](https://confluencesw.t-mobile.com/pages/viewpage.action?pageId=231318210) for working through this issue.
