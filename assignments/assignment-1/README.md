## Assignment 1

**Name of the assignment:** Getting Started on Linear Algebra
**Student Name:** Naman Singh
**Date of submission:** 8th Feb 2026


## How to run code, docker, VS code
1. Start Docker Desktop app.
2. In a terminal window, start the container:
browse to the project directory
Run commands:
docker compose up -d torch.dev.cpu
check running containers using
docker compose ps
3. In VS Code, open the project folder:
Command Palette: Dev Containers: Attach to Running Container…
Select eng-ai-agents-dev
Open folder: /workspaces/eng-ai-agents
OR
In the VS code sidebar go to Remote Explorer and attach the container
4. Start the virtual environment inside the container:
cd /workspaces/eng-ai-agents
make start #Creates the virtual environment
source .venv/bin/activate #Activates the virtual environment
5. Open assignment-1/index.ipynb and assignment-1/low_rank_gaussians_1.ipynb, 
select the kernel from the virtual environment.
6. Run all cells and save the notebooks.
7. Exit the virtual environment by running : deactivate
8. If you are inside the container, exit it by running: exit
9. Stop the docker by running: docker compose stop torch.dev.cpu


