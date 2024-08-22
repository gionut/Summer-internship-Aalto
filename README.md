# Summer-internship-Aalto

[Notion Docs](https://www.notion.so/Devops-Scenario-6828e3b05cde4b75bca50571f9fe6bfc)


# Instructions

In order to run the project, you need to configure another repository containing your application, a `Dockerfile`, the `.gitlab-ci.yml` and the `sonar-project.properties` files.
An example can be found at https://github.com/gionut/gitlab-pipeline-test-repo.

You also need to configure an access token, which you will provide to the `helmfile sync` command as follows:

```shell
./helmfile sync --set repo_token="<REPO_ACCESS_TOKEN"
```

The helmfile sync command will install all helmcharts and run the corresponding jobs. After the command execution completes, your minikube node should have the following kubernetes services, which you need to expose in order to access them via user browser:

```shell
kubectl -n gitlab port-forward svc/gitlab-chart-nginx-ingress-controller 8443:443
kubectl -n gitlab port-forward svc/harbor 8080:443
kubectl -n gitlab port-forward svc/sonarqube-sonarqube 9000:9000
```

## Passwords

To obtain Gitlab password
```shell
# Username is root
kubectl -n gitlab get secret gitlab-chart-gitlab-initial-root-password -o jsonpath="{.data.password}" | base64 --decode
```
Sonarqube: admin admin12345

Harbor: admin Harbor12345

# Troubleshooting

Check the logs of all the jobs for errors:
```shell
kubectl -n gitlab get pods | grep job
```

For the job corresponding to the harbor-chart:
```shell
kubectl -n gitlab get pods | grep robot
```

Check that Gitlab contains the following CI/CD variables:

