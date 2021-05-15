# CMPE 172 - Lab #10 Notes

In this lab, we experiment with CI/CD through GitHub Actions.

## CI

In this portion of the lab, we set up an action to test and build a Gradle-based
project. For this specific action, we trigger it to run on pushes and PRs to
`main`.

The specific action can be found in `.github/gradle.yml`.

![](./images/ci.PNG)


## CD

In this portion of the lab, we set up an action to build a Gradle-based project
and deploy it to GKE.

The specific action can be found in `github/google.yml`.

Before any work, we need to create a service account to allow GitHub to access
our cluster:

![](./images/service-account.PNG)

Then, with details based on our project and the newly made account, we add
secrets at the repo level:

![](./images/actions-secrets.PNG)

Next, we spin up our cluster:

![](./images/initial-cluster.PNG)

At first, I was having trouble getting my build to pass:

![](./images/cd-failure.PNG)

After some googling, I thought my issue was the service account, so I deleted it
and created a new one, this time with owner-level permissions:

![](./images/service-account-2.PNG)

Then, my build failed, but this time, for a different reason! (Progress!)

![](./images/cd-failure-2.PNG)

To fix it, I corrected the name of the deployment (from
`spring-gumball` to `spring-gumball-deployment`), and my build finally passed:

![](./images/cd-success.PNG)

To verify, I checked out my workloads and services/ingress:

![](./images/workloads.PNG)

![](./images/services-and-ingress.PNG)

I then set up a ingress for `spring-gumball-service`:

![](./images/spring-gumball-ingress-setup.PNG)

Then, I verified that I could reach the service:

![](./images/gumball-machine.PNG)

Awesome!