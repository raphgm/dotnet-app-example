# ASP.NET Core .NET Core Application

This ASP.NET Core application uses .NET Core 7.0.

## Prerequisites

- [.NET SDK 7.0](https://dotnet.microsoft.com/download/dotnet/7.0)
- [Docker](https://www.docker.com/get-started)

## Building and Running the Application

- Clone the application with ``git clone url`` 
- Navigate into the root directory with CD  
- Use ``git emote add origin url`` to clone the repo
- Use ``git branch -M main`` to creaete a branch 
-  use `` git push origin main`` to push the code    
    to github.


### Using .NET CLI

1. Restore the dependencies:

    ```sh
    dotnet restore
    ```

2. Build the application:

    ```sh
    dotnet build
    ```

3. Run the application:

    ```sh
    dotnet run
    ```

### Using Docker

1. Build the Docker image:

    ```sh
    docker build -t aspnet-core-dotnet-core .
    ```

2. Run the Docker container:

    ```sh
    docker run -p 80:80 aspnet-core-dotnet-core
    ```

## Push changes to Version control
git push origin main

## CI/CD with GitHub Actions

This project includes a GitHub Actions workflow for CI/CD. The workflow will build and test the application on every push and pull request.

### GitHub Actions Workflow

Create a file named `.github/workflows/ci-cd.yml` with the following content:

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up .NET
      uses: actions/setup-dotnet@v2
      with:
        dotnet-version: '7.0.x'

    - name: Restore dependencies
      run: dotnet restore

    - name: Build
      run: dotnet build --no-restore

    - name: Test
      run: dotnet test --no-build --verbosity normal

    - name: Publish
      run: dotnet publish -c Release -o out

    - name: Build Docker image
      run: docker build -t aspnet-core-dotnet-core .

    - name: Push Docker image
      env:
        DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
        DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
      run: |
        echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
        docker tag aspnet-core-dotnet-core $DOCKER_USERNAME/aspnet-core-dotnet-core:latest
        docker push $DOCKER_USERNAME/aspnet-core-dotnet-core:latest
