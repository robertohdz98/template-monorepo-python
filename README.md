# Monorepo Template

Sometimes, **it is difficult to manage a single repository (i.e., `monorepo`) for a project that contains different modules or components** (e.g., an API, a pipeline, a different service), whose dependencies, libraries or development environment **may be completely different** (and therefore they should be **managed independently**). 

In order to simplify the (complex) task of maintaining several development environments in the same repository, each of them adapted to the needs of each specific module, this monorepo template is presented to support several differentiated modules, using:

- **Dev Container** as development environment
- **Poetry** as dependency management and packaging tool


## Monorepo Structure and Modules

The root directory of the repository will consist of one directory `$moduleName` for each independent component or module (e.g., "module-api", "module-predictor", etc., here as "module-one", "module-two" for simplicity). **The requirements, dependencies and source code for each of the modules are isolated within each of these module-specific directories.**

    │
    ├── .devcontainer                  <--- Definition of several dev containers
    │     ├── module-one
    │     │    └── devcontainer.json
    │     └── module-two
    │          └── devcontainer.json
    │
    ├── module-one
    │     ├── src/package              <--- Source code for indiv component 1        
    |     ├── Dockerfile               <--- Dockerfile for dev container 1
    │     ├── poetry.lock              <--- Poetry dependencies of module 1 (do not edit)
    │     ├── pyproject.toml           <--- Poetry dependencies management of module 1
    │     └── README.md                <--- Description of the specific module 1
    │
    ├── module-two
    │     ├── src/package              <--- Source code for indiv component 2       
    |     ├── Dockerfile               <--- Dockerfile for dev container 2
    │     ├── poetry.lock              <--- Poetry dependencies of module 2 (do not edit)
    │     ├── pyproject.toml           <--- Poetry dependencies management of module 2
    │     └── README.md                <--- Description of the specific module 2
    │
    └── README.md                      <--- Top-level description of the project


## DevContainers customization

Each module in the root directory includes all configurations and specifications about build and customization of the development container in a **devcontainer.json** file in `.devcontainer/$moduleName/devcontainer.json`. 

Here, some customizations for the development environment of the specific module can be defined (e.g., preferred linters, default formatters, workspace mounts, features needed in the development container, etc.). Visual Studio Code extensions can also be added in a specific environment only if needed and not in the other one, and of course even different Python versions specified in the referenced Dockerfile.

The most important JSON fields to be considered (and that must be updated according to each module `$moduleName` to enable devcontainer's full capabilities) are: 

- `"name": "$moduleName-devcontainer"`: the name of the development environment when it is created. It is recommended to set it to the actual name of the module directory in order to be able to easily recognize it at the time of *"Reopen in Container"* (when the available dev containers configured for the several modules will appear):

![img](https://github.com/robertohdz98/template-monorepo-python-project/assets/68640342/6f781204-9d74-4f81-9b59-4fb22ec02e44)


- `"build":{"context": "../../$moduleName",`
`"dockerfile": "Dockerfile"}`
These both options set **(a)** the location of the context folder for building the Docker image, and **(b)** the location of the Dockerfile that defines the contents of the container. Both paths are relative to the folder containing the `devcontainer.json` file, so they must reference the specific module directory of the root directory of the monorepo.

    **NOTE:** The main objective of this approach is to keep the development environments isolated, so that **(a)** no other modules' files are copied into the development environment of a specific module, and **(b)** only the dependencies and libraries strictly necessary for the development of that specific component are installed (managed from $moduleName/pyproject.toml and detailed at its associated $moduleName/poetry.lock).

- `"workspaceFolder": "/app"`: The name of the actual root workspace folder in the dev container, where the source code and files in $moduleName dir will be copied.

- `"workspaceMount": "source=${localWorkspaceFolder}/$moduleName,target=/app,type=bind,consistency=delegated",`: This specifies that the source code and files in $moduleName dir will be mounted in our devcontainer workspace dir "app/".

- **Extensions**:

    - "ms-python.python": linting, debugging, code formatting
	- "ms-python.vscode-pylance": python for vscode
	- "ms-python.flake8", "ms-python.pylint": linting support
	- "ms-python.black-formatter": formatter
	- "ms-python.isort": imports organization support 
	- "ms-toolsai.jupyter", "ms-toolsai.jupyter-renderers": jupyter notebooks support
	- "eamodio.gitlens": code authorship and more git features
	- "usernamehw.errorlens": highlighting of errors
	- "njpwerner.autodocstring": generate docstrings from obj definitions
	- "KevinRose.vsc-python-indent": python indentation support
	- "tamasfe.even-better-toml": TOML language support
	- "redhat.vscode-yaml": YAML language support
	- "yzhang.markdown-all-in-one": markdown files support
	- "GitHub.vscode-github-actions": manage GitHub Actions
	- "wayou.vscode-todo-highlight": highlighting TODO, FIXME annotations
	- "mikestead.dotenv": highlighting dotenv files
	- "ms-kubernetes-tools.vscode-kubernetes-tools": k8s apps support
			


So, this is as simple as opening the specific development environment depending on the specific module in which we are going to develop, and manage from within the dependencies of that component without affecting the others, based on this proposed template.

<br/>

### Add.: References

- https://containers.dev/implementors/spec/#devcontainerjson
- https://github.com/devcontainers/spec/issues/159
- https://docs.github.com/en/enterprise-cloud@latest/codespaces/setting-up-your-project-for-codespaces/adding-a-dev-container-configuration/introduction-to-dev-containers#devcontainerjson
- https://mlops.community/how-to-configure-vs-code-for-ai-ml-and-mlops-development-in-python-%F0%9F%9B%A0%EF%B8%8F%EF%B8%8F/
			