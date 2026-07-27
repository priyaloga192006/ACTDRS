Phase 0 – Development Environment (Day 1)
Step 1: Hardware

Your laptop is enough.

Recommended:

Windows 11 ✅
8 GB RAM (minimum)
20 GB free disk space
Step 2: Install Ubuntu

Since ACTDRS is a Linux project, install Ubuntu.

Option 1 (Recommended)

Ubuntu in VirtualBox

Advantages:

Safe
Easy snapshots
Doesn't affect Windows

Install:

VirtualBox
Ubuntu 24.04 LTS ISO

Create VM:

RAM : 4096 MB

CPU : 2

Storage : 30 GB
Step 3: Update Ubuntu

Open Terminal

Run

sudo apt update

sudo apt upgrade -y
Step 4: Install Python

Check

python3 --version

If not installed

sudo apt install python3
Step 5: Install pip
sudo apt install python3-pip

Check

pip3 --version
Step 6: Install Git
sudo apt install git

Check

git --version
Step 7: Install VS Code
sudo snap install code --classic
Step 8: Create Workspace
mkdir ACTDRS

cd ACTDRS
Step 9: Create Virtual Environment
python3 -m venv venv

Activate

source venv/bin/activate
Step 10: Install Required Libraries
pip install flask

pip install psutil

pip install pandas

pip install numpy

pip install scikit-learn

pip install matplotlib

pip install plotly

pip install requests

pip install sqlalchemy

Later we'll install more.

Your Folder Structure

Create

ACTDRS/

│

├── app/

├── monitor/

├── collector/

├── analyzer/

├── response/

├── dashboard/

├── database/

├── ai/

├── reports/

├── logs/

├── config/

├── tests/

├── docs/

├── README.md

└── requirements.txt

Create using

mkdir app

mkdir monitor

mkdir collector

mkdir analyzer

mkdir response

mkdir dashboard

mkdir database

mkdir ai

mkdir reports

mkdir logs

mkdir config

mkdir tests

mkdir docs
Install Database

For the first version

SQLite

Already included with Python.

No installation needed.

Install Flask

Check

python3

Then

import flask

No error

↓

Good.

Install AI Libraries
pip install scikit-learn

Check

import sklearn
Install Monitoring Library
pip install psutil

Check

import psutil
Install Chart Library

We'll use

Chart.js

No installation required.

We'll link it inside HTML later.

Install Browser

Firefox

Already available.

Install GitHub

Configure

git config --global user.name "Your Name"

git config --global user.email "your@email.com"
Final Environment

After Day 1

Your PC becomes

Windows

↓

VirtualBox

↓

Ubuntu

↓

Python

↓

VS Code

↓

Git

↓

Flask

↓

SQLite

↓

Scikit Learn

↓

ACTDRS Project
Day 2

We don't write AI.

We don't write Dashboard.

We don't write Database.

Instead,

We'll write our first Linux Monitoring Program.

That program will read

CPU
RAM
Disk
Users
Processes

This becomes the Monitor Module.

Day 3

We'll collect

Linux Logs.

Day 4

We'll build

Database.

Day 5

Dashboard.

Day 6

AI.

Day 7

Automatic Response.

What I would change before writing a single line of code

I would spend half a day designing the project before opening VS Code.

Specifically, create these documents first:

System Architecture Diagram – shows every module and how they communicate.
Folder Structure – decide where every file belongs.
Module Specification – one page for each module (Monitor, Collector, AI, Response, Dashboard, etc.) describing its inputs, outputs, and responsibilities.
Data Flow Diagram – how information moves from log collection to AI analysis to response.
Response Policy Document – define exactly which actions are safe to automate (e.g., block an IP, restart a service) and which require human approval.

Professional software teams do this before coding because it prevents confusion later. For a project like ACTDRS, this planning is just as important as the code itself.

Based on everything we've discussed, I think this project is large enough that we should build it like a real product, not a typical college assignment. That means designing first, then implementing one module at a time.
