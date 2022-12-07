View this example in the Dagster docs at https://docs.dagster.io/examples/deploy_docker.

# Cannot reload user code

## Steps to reproduce

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
1. restart the following docker instances
    ```bash
    docker restart docker_example_user_code docker_example_dagit docker_example_daemon
    ```
1. access `http://0.0.0.0:3000/code-locations`
1. reload the repository
1. execute `my_step_isolated_job` [link](http://0.0.0.0:3000/locations/deploy_docker_repository@example_user_code/jobs/my_step_isolated_job)
1. go to `Launchpad` tab
1. click on the `Launch Run` button at the bottom right of the page
1. the result should NOT be successful (unfortunately it is unexpectedly successful)

## successful reload of user code change

Same as above from 1 ~ 7
1. run
    ```bash
    docker stop docker_example_user_code
    docker-compose up --build docker_example_user_code
    ```
1. execute `my_step_isolated_job` [link](http://0.0.0.0:3000/locations/deploy_docker_repository@example_user_code/jobs/my_step_isolated_job)
1. go to `Launchpad` tab
1. click on the `Launch Run` button at the bottom right of the page
1. the result will be unsuccessful as expected (`Exception: Bad io manager`)

