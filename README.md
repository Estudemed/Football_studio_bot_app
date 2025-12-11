name: Fotbool

on:
  workflow_dispatch:
  push:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Install dependencies
        run: |
          sudo apt update
          sudo apt install -y \
            python3 python3-pip python3-setuptools python3-venv \
            openjdk-17-jdk unzip zip git \
            libffi-dev libssl-dev libsqlite3-dev zlib1g-dev \
            libncurses5 libstdc++6 libglu1-mesa

      - name: Install buildozer
        run: |
          pip install --upgrade pip
          pip install cython
          pip install buildozer

      - name: Build APK
        run: |
          buildozer android debug

      - name: Upload APK artifact
        uses: actions/upload-artifact@v3
        with:
          name: football_studio_apk
          path: bin/*.apk
