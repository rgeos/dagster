View this example in the Dagster docs at https://docs.dagster.io/examples/deploy_docker.

# How to reload user code


1. run `docker-compose up`
1. access `http://0.0.0.0:3000/`
1. execute `my_step_isolated_job` [link](http://0.0.0.0:3000/locations/deploy_docker_repository@example_user_code/jobs/my_step_isolated_job)
1. go to `Launchpad` tab
1. click on the `Launch Run` button at the bottom right of the page
1. the result should be successful (as expected)
1. modify method `hello` in `repo.py`
    ```python
    @op
    def hello():
        get_dagster_logger().info(func_name())
        return 2
    ```
1. restart the following docker instance
    ```bash
    docker restart docker_example_user_code
    ```
1. access `http://0.0.0.0:3000/code-locations`
1. reload the repository
1. execute `my_step_isolated_job` [link](http://0.0.0.0:3000/locations/deploy_docker_repository@example_user_code/jobs/my_step_isolated_job)
1. go to `Launchpad` tab
1. click on the `Launch Run` button at the bottom right of the page
1. the result should NOT be successful 

## Things to keep in mind
- volume settings in the `docker-compose` file should be reflected in the `run_launcher` settings of `dagster.yaml` file also
- in this example update with the correct path the following line
    ```bash
    /full/path/to/repo.py
    ```
- see [this example](https://docs.dagster.io/deployment/guides/docker#mounting-volumes) about mounting volumes
