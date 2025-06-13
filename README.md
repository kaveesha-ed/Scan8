Scan8 (Open for GSoC and Hacktoberfest)
========

Scan8 is a distributed scanning system for detecting trojans, viruses, malware, and other malicious threats embedded in files. The system will allow one to submit a list of URLs or files and get the scan results in return.  

## Modules Overview
- **Dashboard**  
  A Flask-based web interface where users can upload files or URLs and track the status of their scans.

- **Coordinator Node**  
  Listens for new scans and pushes tasks to a Redis queue for workers to consume.

- **Worker Node**  

  Continuously listens to the Redis queue, picks up jobs, and performs malware scans using ClamAV.

- **Testing Module**  
  Contains utilities for validating setup and verifying functionality across components.

## Application Architecture
![Scan8 application architecture](https://user-images.githubusercontent.com/54113320/129327795-bd8da18e-484a-428a-aa90-7cc063e11b7f.png)

## Tech Stack & Dependencies
* Language: ```Python 3.8.10```
* Database: ```MongoDB```
* Tools: ```redis-server clamav clamav-daemon```

Each module (Dashboard, Coordinator, Worker) contains its own `requirements.txt` file with Python package dependencies.


## Local Setup Guide

> Ensure you have Python, Git, and Docker installed before starting.

### Step 1 – Clone the repository

```bash
git clone https://github.com/c2siorg/Scan8.git
cd Scan8
```

### Step 2 – Create and activate virtual environment

```bash
python -m venv venv
venv\Scripts\activate  # (on Windows)
```

### Step 3 – Install Python dependencies

```bash
pip install -r Dashboard/requirements.txt
pip install -r Coordinator/requirements.txt
pip install -r Worker/requirements.txt
```

### Step 4 – Create environment file

Ensure a `.env` file exists in the root directory with the following values (defaults are fine):

```
MONGO_HOST=localhost
MONGO_PORT=27017
REDIS_HOST=localhost
REDIS_PORT=6379
```

---

### Step 5 – Start Redis and MongoDB via Docker

```bash
docker run -d --name redis -p 6379:6379 redis
docker run -d --name mongo -p 27017:27017 mongo
```

---

### Step 6 – Start application modules

 Open 3 separate terminals:

**Terminal 1: Dashboard**

```bash
cd Dashboard
set FLASK_APP=app.py
python -m flask run
```

**Terminal 2: Coordinator Node**

```bash
cd Coordinator
python app.py
```

**Terminal 3: Worker Node**

```bash
cd Worker
python app.py
```

Create required folders if not already present:

```bash
mkdir Uploads Results
```



## Usage

Once the system is running:

- Access the dashboard at: [http://127.0.0.1:5000](http://127.0.0.1:5000)
- Submit new scans via the **New Scan** button
- Monitor scan progress in real-time
- View results stored in the `Results/` directory as:
  ```
  <scan_id>_<filename>.json
  ```

## Testing Instructions

To verify your setup and integration:

1. Ensure the `Results/` and `Uploads/` folders are empty
2. Clear MongoDB collections if needed
3. Navigate to the `Testing/` directory
4. Run:

```bash
python app.py -v
```

5. Perform a scan from the dashboard
6. Re-run the test to verify scan completion

---

## Demo videos
* [Introduction](https://drive.google.com/file/d/16oXRxPhDIK1QnPnjoJig_SXpZj8Lj-mq/view?usp=sharing)
* [Application Demo](https://drive.google.com/file/d/1TblQdpIAS4VybZzgb2ORfmJiXs_McVrO/view?usp=sharing)
* [Testing Demo](https://drive.google.com/file/d/1CTvLrD_fSdq6xxXabgkLGOPs3jDLwPrT/view?usp=sharing)

## Contributing

We welcome contributions!  
To contribute:

1. Fork the repository  
2. Create a new branch (`feature/your-feature-name`)  
3. Commit your changes  
4. Push and open a Pull Request


##  License

This project is licensed under the MIT License. See the `LICENSE` file for details.
