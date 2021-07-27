# Example app.yaml files

## Format

There are ordered keys for `pre-build`, `build`, `post-build`, and `run`. The commands/scripts referenced by these keys are executed in-order. If a key is missing, it will be skipped and the next key will be processed. An optional `version` key specifies the version of the `app.yaml` file, if new versions are introduced in the future.

```yaml
version: 1

pre-build: apt-get install xyz

build: pip install requirements.txt
  
post-build: |
  python manage.py makemigrations
  python manage.py migrate
  
run: gunicorn myapp.app --workers 5 --foo bar
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
