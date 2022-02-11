# Delete labels from an issue or a pull request

This `task` can be used to remove labels from a github `pull request` or an `issue`.


## Install the Task

```
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/github-delete-labels/0.1/github-delete-labels.yaml
```

## Parameters

- **GITHUB_HOST_URL**: The GitHub host, adjust this if you run a GitHub enteprise (_default:_`api.github.com`).
- **API_PATH_PREFIX**: The API path prefix, GitHub Enterprise has a prefix (_e.g.:_`api/v3` _default_:`""`).
- **REQUEST_URL**: The GitHub issue or pull request URL where we want to add labels (_e.g._`https://github.com/foo/bar/pull/10`).
- **LABELS**: The actual labels to delete. Multiple labels can be deleted in the form of `array`.
- **GITHUB_TOKEN_SECRET**: The name of the `secret` holding the github-token (_default:_`github`).
- **GITHUB_TOKEN_SECRET_KEY**: The name of the `secret key` holding the github-token (_default:_`token`).


## Secret

* `Secret` to provide Github `access token` to authenticate to the Github.

Check [this](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line) to get personal access token for `Github`.

## Platforms

The Task can be run on `linux/amd64`, `linux/s390x` and `linux/ppc64le` platforms.

## Usage


This task expects a secret named github to exists, with a GitHub token in `token` with enough privileges to delete label from a pull request or an issue.

To delete labels from a pull request or an issue, put all the required params, add required secrets and labels will be deleted from respective pull request or an issue. If a label is not assigned to given pull request or an issue, it will be ignored (albeit a message will be logged).

`Secrets` can be created as follows:
```
apiVersion: v1
kind: Secret
metadata:
  name: github
type: Opaque
stringData:
  token: $(personal_github_token)
```
or

```
kubectl create secret generic github --from-literal token="MY_TOKEN"
```

`Multiple labels` can be deleted in Taskrun as follows:
```
params:
    - name: REQUEST_URL
      value: https://github.com/Divyansh42/aws-cli/pull/1
    - name: LABELS
      value:
        - approve
        - kind/feature
```

[This](../0.1/samples/run.yaml) can be referred to create a Taskrun.
