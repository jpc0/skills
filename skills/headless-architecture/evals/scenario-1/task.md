# Real-time Stock Price Tracker

## Problem/Feature Description

We are building a new stock trading platform. The current stock price feed we receive is incredibly noisy and jittery. This makes the UI feel unstable and difficult for users to read.

We need to implement a feature that receives raw price updates (JSON packets) and displays a **smoothed average** of the last 10 prices. This smoothed value should be updated every time a new price comes in.

The application must be designed to handle high-frequency updates efficiently. The core logic for calculating the average and managing the price history must be robust and separated from any specific UI framework, as we plan to use it across multiple platforms (web, mobile, and desktop).

Please implement this feature, ensuring a clear separation between the data processing logic and the presentation layer.

## Output Specification

The output should include:
1.  A core module that handles raw price updates, maintains a buffer of the last 10 prices, and calculates the smoothed average.
2.  The interface or bridge that exposes the smoothed price to the UI.
3.  A placeholder or example UI component that displays the smoothed price.
4.  Organize your files into a logical directory structure that separates the business logic from the presentation.
