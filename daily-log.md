# 2023-05-07

- Containers like commands
    - You'll want to run commands in a container but like it's on your host machine
    - For example, you don't have python on your machine, you pulled an image with python on it
    - Run the container
        - `docker run -d --name <CONTAINER_NAME> <IMAGE_NAME> sleep infinity`
        - -d runs the container in detached mode
        - --name will 
        - <IMAGE_NAME> can be found by running `docker images`
        - sleep infinity
    - Open your ~/.bashrc file
    - Add these lines
        - `util_container_id='<IMAGE_NAME>'`
        - `alias python='podman exec -it $util_container_id python'`