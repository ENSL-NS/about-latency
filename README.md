# Lab 1 — Getting to know your VM, and measuring the Internet

Lab 1 of the computer networks course at ENSL.

The lab is structured around a single notebook,
[about_latency.ipynb](about_latency.ipynb). You will ping and traceroute RIPE
Atlas anchors around the world and try to predict network latency from
geographical distance.

Everything is meant to be run **inside the Ubuntu VM provided for the course**, to test the working environment. If you plan on using your own machine for the future, it's a good test case. But as said in class, you are responsible for everything a functioning environment.

## 1. Get the code

Open a terminal in the VM and clone the repository:

```bash
git clone https://github.com/ENSL-NS/about-latency.git
cd about-latency
```

### Open it in VS Code

```bash
code .
```

(Or launch VS Code and use *File → Open Folder…* on the cloned directory.)

You need two extensions, both from Microsoft: **Python** and **Jupyter**. Install them from the
Extensions view (`Ctrl+Shift+X`) if they are not already there.

Then open `about_latency.ipynb` and pick a kernel:

1. Click **Select Kernel** in the top-right corner of the notebook.
2. Choose **Python Environments…**.
3. Pick the system Python 3 interpreter (e.g. `/usr/bin/python3`), or your virtualenv if you made
   one.

If no kernel shows up, the Jupyter kernel package is missing — see below.

Run the first cell (Part 0). It checks that every command-line tool and Python library the lab
needs is present. Everything should print `OK`. Anything that prints `MISSING` needs to be
installed.

> The Mininet smoke test (`sudo mn --test pingall`) is run in a terminal, not in the notebook.

## 2. Installing missing Python libraries (Ubuntu)

The VM comes with everything pre-installed, so you should not need this section. If Part 0
reports a `MISSING` library, use it.

The notebook uses `numpy`, `pandas`, `matplotlib` and `scapy`.

### Option A — system packages (recommended on the VM)

```bash
sudo apt update
sudo apt install -y python3-numpy python3-pandas python3-matplotlib python3-scapy
```

For the notebook kernel itself, if VS Code finds no kernel:

```bash
sudo apt install -y python3-pip python3-ipykernel jupyter-core
```

### Option B — pip in a virtual environment

Recent Ubuntu releases refuse `pip install` outside a virtual environment (the
`externally-managed-environment` error). Create one:

```bash
sudo apt install -y python3-venv
python3 -m venv .venv
source .venv/bin/activate
pip install numpy pandas matplotlib scapy ipykernel
```

Then in VS Code, **Select Kernel → Python Environments…** and choose the interpreter inside
`.venv`. Reload the window (`Ctrl+Shift+P` → *Developer: Reload Window*) if it does not appear.

### Missing command-line tools

If Part 0 reports a missing *tool* rather than a library:

```bash
sudo apt update
sudo apt install -y iputils-ping traceroute mtr-tiny dnsutils tshark tcpdump iproute2
```

`tshark` asks whether non-root users may capture packets — answer **Yes**, then log out and back
in (or run `sudo usermod -aG wireshark $USER`) for it to take effect.

If Mininet complains about Open vSwitch:

```bash
sudo service openvswitch-switch start
```

and `sudo mn -c` cleans up a broken Mininet state at any point during the semester.

## 3. A note on the measurements

You are sending packets to machines that belong to other people. Keep the probe counts as they
are in the notebook (20 per target), and do not add random hosts to the target list. The targets
chosen here are RIPE Atlas anchors, which exist precisely to be measured.
