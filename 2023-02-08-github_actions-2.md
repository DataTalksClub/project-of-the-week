## DIY GitHub Actions (Part 2)

* **Goal**: Learn about GitHub Actions and create examples and apply them in your future projects. At the end of the week, you should be familiar with GitHub Actions and specifically its Continious Deployment (CD) part.

* **Dates**: from 8th to 13th February
* **Where:** [`#project-of-the-week`](https://app.slack.com/client/T01ATQK62F8/C02BP4FQH36) in DataTalks.Club (get in slack here: [https://datatalks.club/slack.html](https://datatalks.club/slack.html))

For more information about the "Project of the Week" initiative
at DataTalks.Club, see [README.md](README.md).

If you want to receive reminders about this event, sign up here

<p align="center">
  <a href="https://lu.ma/dtc-potw-diyGA1"><img src="https://user-images.githubusercontent.com/875246/185755203-17945fd1-6b64-46f2-8377-1011dcb1a444.png" height="50" /></a>
</p>


## Technologies 

* Github
* Docker
* Flask,FAST-API
* Unit testing,linting and formating libraries


Note: this is a suggested list of technologies, you can chose
alternatives instead

## Plan

This is a proposed plan only, you don’t have to follow it day-by-day.


### Day 1 (8 February, Wednesday)

* Create a GitHub repository.
* Refresh basic concepts from the suggested material.
* Study the concepts behind Continious Deployment in GitHub Actions tutorial.
* Share your progress in Slack and on social media.

Suggested material:

* 🗒️ [Github Actions Deployment documentation](https://docs.github.com/en/actions/deployment/about-deployments/about-continuous-deployment)
* 📺 [Refresh Github Actions](https://www.youtube.com/watch?v=R8_veQiYBjI)


Found good materials? Create a PR with links!

### Day 2 (9 February, Thursday)

* Build a service (flash or Fast-API) from your Part 1 project. You can copy and run/test the suggested material tutorial.
* If you dont have a project, you can select this example from FAST-API project of the week by Liliana. 
* Also, If you want to create your project from scratch, you can select a dataset from the list bellow.
* Commit your changes.
* Share your progress in Slack and on social media.

Suggested material:

* 📺 [Flask tutoria](https://www.youtube.com/watch?v=Q7ZWPgPnRz8&list=PL3MmuxUbc_hIhxl5Ji8t4O6lPAOpHaCLR&index=52)
* 📺 [Fast-API tutorial (check the FAST-API app part)](https://www.youtube.com/watch?v=h5wLuVDr0oc)
* 💻 [FAST-API sample project](https://github.com/lilianabs/fastapi-car-price-pred)
* 💾 [list of some datasets](https://github.com/DataTalksClub/data-engineering-zoomcamp/blob/main/week_7_project/datasets.md)

Found good materials? Create a PR with links!

### Day 3 (10 February, Friday)

* Prepare a runnable dockerfile of your app. 
* Include an integration test. You can create a docker-compose yaml and include it like the MLops video course suggests.
* Commit your changes.
* Share your progress in Slack and on social media.

Suggested material:

* 📺 [Dockerizing web app (Fast API) video 1](https://www.youtube.com/watch?v=wAtyYZ6zvAs&list=PL3MmuxUbc_hIhxl5Ji8t4O6lPAOpHaCLR)
* 📺 [Dockerizing web app (Fast API) video 2](https://www.youtube.com/watch?v=h5wLuVDr0oc)
* 📺 [Dockerizing web app (Flask))](https://www.youtube.com/watch?v=D7wfMAdgdF8&list=PL3MmuxUbc_hIUISrluw_A7wDSmfOhErJK)
* 💻 [Integration script example:](https://github.com/DataTalksClub/mlops-zoomcamp/blob/main/06-best-practices/code/integraton-test/test_docker.py)
* 🏫 [Integration test tutorial: DataTalksClub MLops Course ](https://www.youtube.com/watch?v=lBX0Gl7Z1ck&list=PL3MmuxUbc_hIUISrluw_A7wDSmfOhErJK)

Found good materials? Create a PR with links!

### Day 4 (11 February, Saturday)

* Finish dockerizing and the integration test.
* Edit the Github workflow to run the image and the integration test. (Now when you make a change to the image it will run a new one).
* Commit your changes.
* Share your progress in Slack and on social media.

Suggested material:

* 🗒️ [Github Actions creating docker container](https://docs.github.com/en/actions/creating-actions/creating-a-docker-container-action)
* 🏫 [Integration test tutorial: DataTalksClub MLops Course ](https://www.youtube.com/watch?v=lBX0Gl7Z1ck&list=PL3MmuxUbc_hIUISrluw_A7wDSmfOhErJK)
* 💻 [Example workflow](https://github.com/hassan11196/ML-model-ship-and-deploy/blob/master/.github/workflows/ci.yaml)

Found good materials? Create a PR with links!

### Day 5 (12 February, Sunday)

* Edit the workflow so that pushed the image to your private docker repository https://hub.docker.com/. Make sure you dont publish your secrets (username and password!).
* Commit your changes.
* Share your progress in Slack and on social media.

Suggested material:

* 📺 [Github Actions Tutorial (check the last part of the video)](https://www.youtube.com/watch?v=R8_veQiYBjI)
* 💻 [Example script for pushing to private docker repo](https://github.com/nanuchi/my-project/blob/master/.github/workflows/ci.yml)


Found good materials? Create a PR with links!

### Day 6 (13 February, Monday)

* Study how you can deploy your service to a cloud provider (Heroku, AWS,Google cloud, Azure). 
* Continue exploring more about this topic.
* Polish the documentation for your project.
* Push your changes to GitHub.
* Share your progress in Slack and on social media.
* Give us feedback.
* Add the link to your project to this project of the week GitHub page.

Suggested material:

* 🗒️ [Deploy a machine learning model with Fastapi docker and github actions](https://towardsdatascience.com/how-to-deploy-a-machine-learning-model-with-fastapi-docker-and-github-actions-13374cbd638a)

Found good materials? Create a PR with links!


Datasets

* 💾 [list of some datasets for your project](https://github.com/DataTalksClub/data-engineering-zoomcamp/blob/main/week_7_project/datasets.md)

**Materials legend**:

* 🏫 Course
* 💾 Dataset
* 🗒️ Article
* 📺 Video tutorial
* 💻 Code

## Projects

List of projects from our participants:

* Project link 1
* Project link 2
* ...
* (Create a PR)

(We will put the projects here after the event finishes)
