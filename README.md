# Overview

This is a reproducer to reproduce an annoying VS Code pylance bug where intellisense does not update for changes in python files.

# Setup

1. Clone this repo
2. Run below commands to install a venv

```bash
cd pylance_issue
python3 -m venv venv
```

# Steps to reproduce the issue

1. Open app_one.code-workspace in vscode
2. Open the test.py file
3. Hover over the class to get the below docstring messages

```
from app.one.one import AppOneTest # Should show "AppOneTest."
from app.two.two import AppTwoTest # Should show "AppTwoTest."
```

4. Go to AppOneTest file and update the docstring to something else
5. Go to AppTwoTest file and update the docstring to something else
6. Go back to the test.py file
7. Hover over the class to get the docstring

Actual: Docstring not updated for app.two.two.AppTwoTest, but updated for app.one.one.AppOneTest

Expected: Docstring for both to be updated.

# Further info

1. If you open app_two.code-workspace in vscode which only contains a single folder and go through the same test steps, the correct docstring is shown after update.

2. In the logs for app_one workspace, i could see multiple services being started in the language server logs, and when i update a file i can see the fschange triggering.

3. I think the issue maybe that not all services are being updated with the information.

4. With only one service in the app_two workspace you do not run into this issue. I am not however sure how it determines which service to use for the auto complete. i guess it is dependent on where the python file is located.

# Versions

Reproduced this in

Python 3.9.6 on MacOS using

1. vscode 1.85
2. pylance v2024.2.2
3. python v2024.0.1
4. jupyter v2023.11.1100101639
