#!/bin/bash

# Self-Healing AI Pan-Tilt Setup Script
# Author: Ralph's Raspberry Pi Genius Assistant
# Description: Automatically sets up the Raspberry Pi for AI Camera and Pan-Tilt HAT, ensuring all dependencies are installed and working.

echo "Starting Self-Healing AI Pan-Tilt Setup..."
echo "------------------------------------------"

# Step 1: Update the System
echo "Updating system packages..."
sudo apt update && sudo apt upgrade -y

# Step 2: Enable I2C
echo "Ensuring I2C is enabled..."
if ! grep -q "dtparam=i2c_arm=on" /boot/config.txt; then
  echo "Enabling I2C..."
  echo "dtparam=i2c_arm=on" | sudo tee -a /boot/config.txt
  echo "I2C enabled. A reboot may be required for changes to take effect."
else
  echo "I2C is already enabled."
fi

# Step 3: Install System Dependencies
echo "Installing system dependencies..."
sudo apt install -y python3-pip python3-venv i2c-tools python3-libcamera libcamera-dev

# Step 4: Create and Activate Python Virtual Environment
echo "Setting up Python virtual environment..."
rm -rf ai_env
python3 -m venv --system-site-packages ai_env
source ai_env/bin/activate

# Step 5: Upgrade pip and Install Required Python Libraries
echo "Installing Python dependencies..."
pip install --upgrade pip
pip install pantilthat picamera2 numpy pillow

# Step 6: Test Pan-Tilt HAT
echo "Testing Pan-Tilt HAT..."
python3 -c "import pantilthat; print('Pan-Tilt HAT is working!')" || echo "Error: Pan-Tilt HAT test failed. Check wiring or installation."

# Step 7: Test AI Camera
echo "Testing AI Camera..."
python3 -c "from picamera2 import Picamera2; print('AI Camera is working!')" || echo "Error: AI Camera test failed. Check camera connection or installation."

# Step 8: Final Output
echo "------------------------------------------"
echo "Setup completed. If any errors occurred, check the output above."
echo "You may need to reboot the system for all changes to take effect."
# self-healing-ai-setup
