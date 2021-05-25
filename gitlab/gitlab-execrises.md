## GitLab Exercises

### Exercise 1

1.Create new gitlab repo and .gitlab-ci.yml file with two stages as shown.  
make sure in stages you have 2 stages : build and test.

in build section:

- connect to related stage.
- script should include creating build dir, cd to build, create car.txt, and echo the related parts into the file.
- don't forget to add artifacts to this section and add to paths the related directory.

in test section:

- connect to related stage.
- script should include testing if file exits, cd into build dir, and test if strings are appearing in car.txt file

Remember - GOOGLE is your FRIEND.

### Exercise 2

1.Let's extend the pipeline with a new stage that does some preparation work, such as creating the folder and the car.txt file.

2.Add a new job called "prepare the car".

Add the following scripts:

    - mkdir build

    - cd build

    - touch car.txt

Remove the command mkdir / touch from the "build the car" job.

3.Your pipeline should now fail.

4.Find out why its failing and fix the issue.

5.Let's create during the build process a file with your name, to indicate who has created the file.
Create a new job called "author" and assign it to the build stage.

In the script section add the following:

    - mkdir meta

    - cd meta

    - echo $GITLAB_USER_NAME > author.txt

Make sure this job saves the meta/author.txt file as an artifact.

7.Look at the pipeline. You should notice that the author and the build the car jobs will be executed in parallel. This is because you have assigned two jobs to the same stage.

8.Look at the artifacts inside the meta folder. You should find your name inside the file author.txt

### Exercise 3

1. Let's extend the pipeline with a new stage that does some testing.

2. First we will need to use the artifacts from the first stage.

3. Lets create new stage for testing the website files.

4. Test and make sure your index.html file is exists.

5. Time your CI process - how much time does it take ? check duration.

6. change your test stage to use alpine image. did it make any difference in the duration?

7. add a job and connect it also to to test stage.

8. this job should npm i dependencies and run 'npm run test' - see what happens in logs.

9. add at least 3 more test for files (check if exits).

10. Bonus - find online how to add test to the react app, add 3 more tests, commit and check that all tests pass(the test itself does not matter - make it check that 1+1 = 2 )

11. show to class member to approved & send to me in slack to check.

### Exercise 4

1. Let's extend the pipeline with Deploy stage.

2. Open your command line/shell in your project /build folder and enter 'surge token' - we will use it in the next step.

3. Go to settings => ci/cd => variables and add:

- Key: SURGE_TOKEN , Value: the token from step 2.
- Key: SURGE_LOGIN, Value: your email address you using in surge.  
  save it.

4. Add a deploy website job.

5. install surge globally as part of the script (you can use the command we use when we try it locally)

6. add to script the command 'surge --project ./build --domain $your-domain'

`CHANGE $your-domain to your surge domain. e.g ben.surge.sh`

7. Save & test your pipeline is running successfully.

8. Open your project and find src/App.js file

9. change ` <p> Edit <code>src/App.js</code> and save to reload. </p>`

   to your name. for example `<p> ben </p>`

10. run `npm run start` and see if your see yor name in your screen ! yay !

11. commit & push => your pipeline and your ci/cd should run.

12. test if your website is updated in your domain.

### Exercise 5

1. Let's Create new stage called deploy tests

2. create a new job using alpine image.

3. add a script to test if 'react' string exists (you can use also curl command). GOOGLE IS YOUR FRIEND.

4. run and test your pipeline.

5. something went Wrong? find out why. it is possible you need to change your image to an image that supports curl command?

### Exercise 6

1. Add cache to your pipeline - use ${COMMIT_REF_SLUG} as key.

2. Not every job in our pipeline need the dependencies (no need for cache) - disable cache inside jobs that does not need it. (you can cancel using cache:{})

3. Check the time of pipeline - did you see big difference ?

### Exercise 7

The default cache behavior in Gitlab CI is to download the files at the start of the job execution (pull), and to re-upload them at the end (push). This allows any changes made by the job to be persisted for future runs (known as the pull-push cache policy).  
 Gitlab offers the possibility of defining how a job should work with cache by setting a policy. So if we want to skip uploading cache file, we can use the setting in the cache configuration.

In Gitlab CI it is possible to create jobs that are only executed when a specific condition is fulfilled. For example if we want to run a job only when the pipeline is triggered by a schedule, we can configure it with:

```
 only:
- schedules
```

The same goes the other way around. If you don't want to run a job when the pipeline is triggered by a scheduled run, simply add to the respective jobs:

```
except:
    - schedules
```

Let's create a job that runs only once per day and updates the cache. The job will not need to download the caches (pull). It only needs to create new caches (push).

For this do the following:

- create a stage called "cache"

- create a job called "update cache"

- make sure the job does runs the command npm install (to install all dependencies)

- add the following cache policy to the job: policy: push. Note that you will need to define the entire cache configuration (as you have done globally). You can not override only the policy.

- make sure the "update cache" job only runs when the pipeline is triggered by a schedule

- make sure that all other jobs DO NOT RUN when the pipeline is triggered by a schedule

- create a new scheduled pipeline run and make it run once per day

- manually run the scheduled pipeline and inspect the pipeline (only one job should be displayed)

- manually the pipeline (the "update cache" job should not appear in the pipeline)

- Send me in slack the yml file to check if you did all ok.
