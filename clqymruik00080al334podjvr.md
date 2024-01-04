---
title: "Python for DevOps"
datePublished: Thu Jan 04 2024 03:11:27 GMT+0000 (Coordinated Universal Time)
cuid: clqymruik00080al334podjvr
slug: python-for-devops

---

Setting up a Python environment on Ubuntu is pretty straightforward. Here are the basic steps:

1. **Check if Python is installed:** Ubuntu usually comes with Python pre-installed. You can check the version by running:
    
    ```dockerfile
    python3 --version
    ```
    
2. **Install Python:** If Python is not installed or you want to install a specific version, use the package manager `apt`:
    
    ```dockerfile
    sudo apt update
    sudo apt install python3
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704337212490/f54d8358-d356-4b13-af27-15d692561b7e.png align="center")
    
3. **Install pip (Python package manager):**
    
    ```dockerfile
    sudo apt install python3-pip
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704337378892/0b186871-b666-4ccc-860b-ad886101bb35.png align="center")
    
4. **Virtual Environment (Optional but highly recommended):** Setting up a virtual environment helps isolate your Python projects. Install `venv`:
    
    ```dockerfile
    sudo apt install python3-venv
    ```
    
5. **Create a virtual environment:** Navigate to your project directory and create a virtual environment:
    
    ```dockerfile
    python3 -m venv venv
    ```
    
6. **Activate the virtual environment:**
    
    ```dockerfile
    source venv/bin/activate
    ```
    
    Your terminal prompt should change, indicating the virtual environment is active.
    
7. **Install packages using pip:** Now, you can install Python packages using `pip` within your virtual environment.
    
    For example:
    
    ```dockerfile
    pip install "package_name"
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704337664956/a45a7bda-54e3-47df-af0a-3dfd7e8731a0.png align="center")
    
8. **Deactivate the virtual environment:** When you're done working on your project, deactivate the virtual environment:
    
    ```dockerfile
    deactivate
    ```
    

These steps should get you started with a basic Python environment on Ubuntu. If you have specific project requirements, you might need additional libraries or tools.

If you've set up your Python environment, you can save this line in a file with a `.py` extension, for example, [`filename.py`](http://hello.py), and then run it using the command:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704337750003/cdfb5fa8-686c-4ae3-b4e4-a1e654752914.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704337864030/1ee773c0-4b67-4076-a4c3-20d7c999edaf.png align="center")