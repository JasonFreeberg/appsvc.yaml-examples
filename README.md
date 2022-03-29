# Example appsvc.yaml files

The App Service YAML file (`appsvc.yaml`) is used to specify the build and run commands for your Web Apps, thus overriding any defaults provided by the App Service Build Service. App Service expects this file to be in the root directory of your project. As of March 2022, only the "pre-build" and "post-build" commands are implemented.

## Format

There are ordered keys for `pre-build`, `build`, `post-build`, and `run`. The commands/scripts referenced by these keys are executed in-order. If a key is missing, it will be skipped and the next key will be processed. An optional `version` key specifies the version of the `app.yaml` file, if new versions are introduced in the future.

```yaml
version: 1

pre-build: apt-get install jq

build: pip install requirements.txt
  
post-build: |
  python manage.py makemigrations
  python manage.py migrate
  
run: gunicorn myapp.app --workers 5
```

## Open questions

- How are virtual envs handled? Should the user create it? If so, do they need to re-activate the venv in the `post-build` commands?
- Would it be advantageous to allow metadata in the values like specifying a working directory?
  ```
  post-build:
    dir: home/site/deployments/tools/
    timeout: 2m
    run: |
      bash foo bar
      wget http://foo.bar.com > my-downloaded-tool.zip
  ```

## Notes

The JBoss and Tomcat runtimes don't need "run" keys because those runtimes will identify the relevant artifacts and run them. But the build keys would be helpful for configuring the Tomcat server
