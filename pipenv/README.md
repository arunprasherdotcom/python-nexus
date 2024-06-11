# Pipenv

`pipenv` is a tool that helps manage Python project dependencies and virtual environments. It aims to bring the best of all packaging worlds (bundled dependencies, development dependencies, etc.) to the Python world.

Here's a basic rundown of what it does:

1. **Dependency Management**: `pipenv` automatically manages project dependencies specified in a `Pipfile`. It keeps track of which packages your project depends on and what versions of those packages should be used.

2. **Virtual Environments**: It automatically creates and manages a virtual environment for your Python project. This keeps your project's dependencies isolated from the system Python installation and other projects.

3. **Locking Dependencies**: `pipenv` generates a `Pipfile.lock` file, which ensures deterministic builds by locking dependencies to specific versions. This helps in reproducing the same environment across different systems.

4. **Installation and Removal**: `pipenv` provides commands to install and remove dependencies from your project. These commands also update the `Pipfile` and `Pipfile.lock` accordingly.

5. **Shell Integration**: `pipenv` has a shell command that allows you to spawn a subshell within the virtual environment, making it easy to work within the context of your project.

Overall, `pipenv` simplifies the management of Python projects by providing a unified tool for dependency management and virtual environment handling.


## Pipenv General Commands

1. **Install Packages**: Install packages specified in the `Pipfile`:
   ```sh
   pipenv install
2. **Install Specific Package**: Install a specific package and add it to the Pipfile:
   ```sh
   pipenv install package_name
3. **Install Development Packages**: Install development packages (e.g., testing libraries) and add them to the Pipfile:
   ```sh
   pipenv install --dev
4. **Uninstall Package**: Remove a package from the virtual environment and the Pipfile:
   ```sh
   pipenv uninstall package_name
5. **Create a Virtual Environment**: Create a new virtual environment with a custom name:
   ```sh
   PIPENV_VENV_IN_PROJECT=custom_name pipenv --python 3.8.10
6. **Rename .venv**: Rename the Virtual Environment Directory (Optional):
   ```sh
    mv .venv custom_name
   ```
7. **Activate the Virtual Environment**: Enter the virtual environment shell:
    ```sh
    pipenv shell --venv custom_name
8. **Run a Command in the Virtual Environment**: Execute a command within the virtual environment without activating it:
    ```sh
    pipenv run python3.9 script.py
9. **Check Security Vulnerabilities**: Check for security vulnerabilities in installed dependencies:
    ```sh
    pipenv check
10. **Update Packages**: Update packages to their latest versions:
    ```sh
    pipenv update --venv custom_name
11. **Generate requirements.txt**: Generate a requirements.txt file from the Pipfile.lock:
    ```sh
    pipenv lock -r

## Pipfile vs requirements.txt

1. **Structure**:
   - **Pipfile**: Written in TOML format, supports multiple sections like `[packages]` and `[dev-packages]`.
   - **requirements.txt**: Simple text file listing package names, often with version constraints.

2. **Version Specification**:
   - **Pipfile**: Offers flexible version specification including ranges and wildcards.
   - **requirements.txt**: Typically uses operators like `==`, `>=`, or `~=`, less flexible than Pipfile.

3. **Usage**:
   - **Pipfile**: Managed by `pipenv`, used for dependency and virtual environment management.
   - **requirements.txt**: Widely used with standard `pip` tool for installing dependencies directly.

4. **Locking Dependencies**:
   - **Pipfile**: Generates a `Pipfile.lock` for locking dependencies to specific versions.
   - **requirements.txt**: Doesn't inherently lock dependencies, but can be achieved with tools like `pip-tools`.

## In summary, Pipfile and requirements.txt serve different purposes:

1. **Pipfile**:
   - More feature-rich with flexible versioning and locking mechanisms.
   - Managed by pipenv for dependency and virtual environment management.

2. **requirements.txt**:
   - Simpler format commonly used with standard pip for direct dependency installation.

3. **Installation Difference**: The installation of dependencies varies between using a `requirements.txt` file and `pipenv`:
   - `pip` directly installs dependencies listed in a `requirements.txt` file.
   - `pipenv` creates and manages a virtual environment along with installing dependencies specified in a `Pipfile`.
   - To install dependencies from a `requirements.txt` file using `pipenv`, you can use the command `pipenv install -r path/to/requirements.txt`.

<br/><br/>

# Managing Dependencies with Pipenv in a Django Application

## Creating a Pipfile and Pipfile.lock

1. **Install `pipenv`:** First, ensure that `pipenv` is installed. You can do this by running `pip install pipenv`.

2. **Create the `Pipfile`:** Save the required package configurations in a file named `Pipfile` in your project directory. Include both the `[packages]` and `[dev-packages]` sections if necessary. Ensure that you include the necessary source information, such as the URL and name of the package index, within the `[[source]]` section.

3. **Install Packages and Generate `Pipfile.lock`:** Run `pipenv install` in your project directory. This command reads the `Pipfile`, installs the specified packages and their dependencies, and generates a `Pipfile.lock` file. If any package versions are specified in the `Pipfile`, `pipenv` installs those exact versions; otherwise, it installs the latest compatible versions.

4. **Updating `Pipfile.lock`:** Whenever you modify the `Pipfile`, you can run `pipenv lock` to update the `Pipfile.lock` file based on the changes. This ensures that the `Pipfile.lock` file accurately reflects the current state of your project's dependencies.


## Example Pipenv File

```toml
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
django = "==3.1.4"
celery = "*"
sentry-sdk = "==1.3.1"
python-decouple = "==3.5"
pymongo = "*"
psycopg2-binary = "==2.8.6"
djangorestframework = "==3.12.2"
django-allauth = "==0.44.0"
django-cors-headers = "==3.6.0"
django-filter = "==2.4.0"
django-extensions = "==3.1.3"
django-sslserver = "==0.22"
django-resized = "==0.3.11"
Pillow = "==8.1.0"
djangorestframework-simplejwt = "==4.6.0"
stripe = "==2.56.0"
geopy = "==2.1.0"
paypal-checkout-serversdk = "==1.0.1"
pysftp = "==0.2.9"
docusign-esign = "==3.7.1"
xmltodict = "==0.12.0"
sendgrid = "==6.9.7"
twilio = "==7.4.0"
json2html = "==1.3.0"
xhtml2pdf = "==0.2.5"
django-datatables-view = "==1.19.1"

[dev-packages]
mypy = "*"
pytest = "*"
pytest-asyncio = "*"
pytest-mock = "*"
pytest-cov = "*"

[requires]
python_version = "3.10"

[pipenv]
allow_prereleases = true
```

## Explanation

The provided Pipenv file includes configurations for managing dependencies in a Django application. Here's a brief explanation of the key components:

- **[[source]]:** Specifies the source for fetching packages. In this case, it's the Python Package Index (PyPI) at `https://pypi.org/simple`. SSL verification is enabled (`verify_ssl = true`).

- **[packages]:** Lists the main packages required for the Django application along with their specific versions.

- **[dev-packages]:** Lists the development packages needed for tasks like testing, linting, etc.

- **[requires]:** Specifies the required Python version for the project.

- **[pipenv]:** Additional configuration options for `pipenv`, such as allowing pre-releases.

By following this structure and including the necessary package configurations, you can effectively manage dependencies in your Django project using Pipenv.


## Automatic Updating of `Pipfile` and `Pipfile.lock`

- When you install a new package using `pipenv`, both the `Pipfile` and `Pipfile.lock` files are automatically updated to reflect the newly installed package and its dependencies.
  
- `pipenv` updates the `Pipfile` to include the new package in the `[packages]` section, with its specific version if provided, or with the latest compatible version.

- Dependency resolution is performed to ensure compatibility, and the `Pipfile.lock` file is updated accordingly to reflect the resolved dependencies and their specific versions.

By following these steps, you can manage your project's dependencies effectively using `pipenv`, ensuring consistency and reproducibility across different environments.


## Setting Up and Completing the Project with Pipenv

If you have an existing `Pipfile` and `Pipfile.lock` file in your project, follow these steps to set up and complete the project process:

1. **Install Pipenv**: Ensure `pipenv` is installed. You can install it using `pip`.
    ```sh
       pip install pipenv
2. **Install Dependencies**: Use `pipenv` to install the project's dependencies as specified in the `Pipfile` and `Pipfile.lock`. This command will create a virtual environment (if one does not already exist), install all the packages listed in the `Pipfile.lock`, and ensure that the exact versions specified are used.
    ```sh
        pipenv install

3. **Activate the Virtual Environment**: Once the dependencies are installed, activate the virtual environment created by `pipenv`. 
    ```sh
        pipenv shell
This will drop you into a shell where the virtual environment is active, and you can run your project's scripts and commands.

4. **Run Your Project**: With the virtual environment active, you can now run your project as you normally would. For a Django application, this might include running migrations and starting the development server.
    ```sh
        python manage.py migrate
        python manage.py runserver

5. **Installing New Packages**: If you need to add new packages to your project, use `pipenv install <package_name>` for production packages or `pipenv install --dev <package_name>` for development packages. This will automatically update both the `Pipfile` and `Pipfile.lock`.
    ```sh
        pipenv install requests

6. **Updating Dependencies**: To update your dependencies to the latest allowed versions as specified in your `Pipfile`, use the `pipenv update` command.
    ```sh
        pipenv update

7. **Checking Dependency Security**: You can check for any known security vulnerabilities in your dependencies by running the `pipenv check` command.
    ```sh
        pipenv check
8. **Generating Requirements File**: If you need to generate a `requirements.txt` file from your `Pipfile.lock` for compatibility with tools that require it, you can use the `pipenv lock -r > requirements.txt` command.
    ```sh
        pipenv lock -r > requirements.txt
    By following these steps, you ensure that your project's dependencies are managed consistently and securely using `pipenv`.

<br/><br/>
# Difference Between Pipenv and Pyenv

Pipenv and pyenv are tools used in Python development, but they serve different purposes and have different functionalities. Here’s a breakdown of the differences:

## Pipenv

**Purpose:**
- Pipenv is designed to manage project dependencies and virtual environments.

**Key Features:**
1. **Dependency Management:**
   - Pipenv uses a `Pipfile` to specify project dependencies, replacing the traditional `requirements.txt`.
   - It uses a `Pipfile.lock` to ensure deterministic builds, specifying the exact versions of dependencies to be installed.

2. **Virtual Environment Management:**
   - Pipenv automatically creates and manages a virtual environment for your projects.
   - It isolates project dependencies from the global Python environment, ensuring that each project has its own set of dependencies.

3. **User-Friendly:**
   - Pipenv combines the functionalities of `pip` and `virtualenv` (or `venv`) into a single tool, making dependency management more straightforward.

4. **Security:**
   - Pipenv checks for security vulnerabilities in the dependencies.

**Usage:**
- Common commands include:
  - `pipenv install` – Installs dependencies from the `Pipfile`.
  - `pipenv install <package>` – Adds a package to the `Pipfile` and installs it.
  - `pipenv shell` – Spawns a shell with the virtual environment activated.

## Pyenv

**Purpose:**
- Pyenv is designed to manage multiple Python versions on a single machine.

**Key Features:**
1. **Python Version Management:**
   - Pyenv allows you to easily switch between multiple versions of Python.
   - It lets you install multiple Python versions and set global, local (per-project), and shell-specific Python versions.

2. **Flexibility:**
   - Pyenv can be combined with other tools like `pipenv` or `virtualenv` to manage virtual environments and dependencies for different Python versions.

3. **Isolation:**
   - By isolating Python versions, pyenv helps avoid conflicts between projects that require different Python versions.

**Usage:**
- Common commands include:
  - `pyenv install <version>` – Installs a specific Python version.
  - `pyenv global <version>` – Sets the global Python version.
  - `pyenv local <version>` – Sets the Python version for the current directory.
  - `pyenv shell <version>` – Sets the Python version for the current shell session.

## Summary

- **Pipenv**: Focuses on managing project dependencies and virtual environments, ensuring that each project has its own isolated environment with a specified set of dependencies.
- **Pyenv**: Focuses on managing multiple Python versions on a single machine, allowing you to switch between different versions easily.

These tools can be used together: pyenv to manage Python versions and pipenv to manage dependencies and virtual environments for projects. This combination can provide a robust setup for Python development, allowing you to maintain clean, isolated environments tailored to each project’s specific requirements.

<br/><br/>
# How Pipenv Determines Python Version

Pipenv determines which Python version to use through several mechanisms:

## 1. Pipfile Configuration

In the `Pipfile`, you can specify the Python version required for your project. Pipenv will use this information to create a virtual environment with the specified Python version if it is available on your system.

## 2. Pipenv Environment Variables

You can set the `PIPENV_PYTHON` environment variable to specify the path to the Python interpreter that Pipenv should use. This allows you to override the Python version specified in the `Pipfile` or to set a version if it is not specified.

## 3. pyenv Integration

If you are using pyenv to manage Python versions, Pipenv can use the version of Python set by pyenv. Pipenv will look for the Python version specified by pyenv when creating the virtual environment. This can be particularly useful when you have different projects requiring different Python versions.

## 4. Global Python Interpreter

If no specific Python version is specified in the `Pipfile` or via environment variables, and if pyenv is not used, Pipenv will fall back to the system’s default Python interpreter.

## How Pipenv Determines Python Version

When you run `pipenv install`, Pipenv follows this order to determine which Python version to use:

1. **PIPENV_PYTHON environment variable**: If this is set, Pipenv will use this Python interpreter.
2. **Pipfile's [requires] section**: Pipenv will use the Python version specified here.
3. **pyenv**: If pyenv is installed and configured, Pipenv will use the version specified by pyenv for the current project directory.
4. **Global Python interpreter**: If none of the above is set, Pipenv will use the system’s default Python interpreter.

## Example Workflow

1. **Specify Python Version in Pipfile**.
    ```sh
    [requires]
    python_version = "3.8"
2. **Install Project Dependencies**.
    ```sh
    pipenv install
Pipenv will then:
- Check if `PIPENV_PYTHON` is set.
- Look at the `Pipfile` to see the required Python version.
- Check pyenv for the specified version.
- Use the system’s default Python interpreter if no specific version is found.

By using these methods, Pipenv ensures that the correct Python version is used for each project, helping maintain compatibility and avoid conflicts.

<br/><br/>
# Can Pipenv install the required Python version like pyenv does with the .nvmrc file for Node.js?

Pipenv determines which Python version to use through several mechanisms:

## 1. Pipfile Configuration

In the `Pipfile`, you can specify the Python version required for your project. Pipenv will use this information to create a virtual environment with the specified Python version if it is available on your system.

Example `Pipfile`:
    ```toml
    [requires]
    python_version = "3.8"

## Using an Existing Python Version

When you run `pipenv install`, Pipenv will look for the specified Python version on your system. If it’s not available, Pipenv will not automatically install it for you. You would need to ensure that the specified Python version is installed on your system, potentially using pyenv or another version management tool.

### Example Workflow with Pyenv and Pipenv

To achieve a workflow similar to `.nvmrc` with nvm for Node.js but using pyenv and Pipenv:

1. **Install pyenv** (if not already installed):
   ```sh
   curl https://pyenv.run | bash
   ```

2. **Create a .python-version file specifying the Python version required:**
   ```sh
   echo "3.8.10" > .python-version
   ```
3. **Install the specified Python version with pyenv:**
    ```sh
    pyenv install 3.8.10
    ```


4. **Set the local Python version with pyenv:**
    ```sh
    pyenv local 3.8.10
    ```

5. **Create a Pipfile with the required Python version:**
    ```sh
    [requires]
    python_version = "3.8"
6. **Install dependencies with Pipenv:**
   ```toml
   pipenv install
   ```

    ### Automated Setup Script
    
    Here’s an example of a script to automate this setup:
    
    ```sh
    # Ensure pyenv is installed
    if ! command -v pyenv &> /dev/null; then
        curl https://pyenv.run | bash
        export PATH="$HOME/.pyenv/bin:$PATH"
        eval "$(pyenv init --path)"
        eval "$(pyenv init -)"
    fi
    
    # Specify Python version
    PYTHON_VERSION="3.8.10"
    
    # Create .python-version file
    echo $PYTHON_VERSION > .python-version
    
    # Install Python version with pyenv
    pyenv install -s $PYTHON_VERSION
    
    # Set local Python version
    pyenv local $PYTHON_VERSION
    
    # Create Pipfile if it doesn't exist
    if [ ! -f "Pipfile" ]; then
        pipenv --python $PYTHON_VERSION
    fi
    
    # Install dependencies
    pipenv install
    ```

## Summary
- pyenv is used to manage and install different Python versions and can automatically switch versions based on configuration files like .python-version.
- Pipenv is used to manage project dependencies and virtual environments, and it relies on the specified Python interpreter.
- Pipenv does not install Python versions; it uses the versions that are already installed on your system.
- To automate Python version management similar to .nvmrc, you should use pyenv in conjunction with Pipenv.

