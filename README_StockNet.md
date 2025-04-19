# StockNet 📊🌐

**StockNet is a multi-approach, feature-rich, and user-friendly stock prediction platform that serves as a tool in your investing/trading decisions.** It features functions like **Price or Movement Predictions** and **Extensive or Specific Index Search**, with **built-in Data Visualization Tools** which provides powerful insights for **traders/investors**.

> Just like how LLMs become better by training on larger datasets, the accuracy of StockNet's inferences may increase with the daily addition of backlogs of historical data.

![StockNet Demo](https://shoterz.github.io/TOOTURNT/StockNet_Demo.gif)



## Layout Breakdown 🗂️

- 🖥️ **Main Window**: Select a ticker and pick `Approximate Logic`, `Group Generalisation` or none for `Conventional Rounding`. (For more information, check the footer)
Be sure to check on `Exclude Last Row` if trading is currently in session or it will interfere with the results.  

> `buying_volume` means there is an increase in trading volume and the price closed higher from the previous day. `selling_volume` means there is an increase in trading volume and the price closed lower from the previous day. If the trading movement did not increase from the previous day it will return `neutral`. These are just derivative stats and not the actual prediction.

- 🗃️ **1-D GPU Inference**: Run the logic with `Process the data`. If you changed a ticker in **Main Window**, you will need to re-run.

- 🗄️ **2-D GPU Inference**: Runs the process automatically when the page is clicked.

- 🔎 **Filter and Search**: Allows user to search up a specific index or a range.

- 📊 **Stock Data Visualization**: Provides insightful and interactive visual representations of market trends, enabling better analysis and decision-making.
  
> **100** is a substitute for digit **0** as the digit zero causes compiling errors during runtime

### Data Collection Approaches 📟

#### **Conventional Rounding**:
Each value x in `pct_changes` is processed by the following rules:

    For values between -1 and 0:

        Return -1

    For values between 0 and 0.5:

        Return 100

    For values between 0.5 and 1:

        Return 1

    For all other values:

        Return the rounded value of x

#### **Approximate Logic**:
📉 Negative Changes:

    -8.5 < x <= -6.5 → -7

    -10.5 < x <= -8.5 → -9

    -13.5 < x <= -10.5 → -11

    -16.5 < x <= -13.5 → -14

    -20.5 < x <= -16.5 → -17

    -25.5 < x <= -20.5 → -21

    -30.5 < x <= -25.5 → -26

    -49.5 < x <= -30.5 → -31

    -99.99 < x <= -49.5 → -50

⚠️ Near Zero:

    -1 < x < 0 → -1

    0 <= x < 0.5 → 100 (acts as a symbolic placeholder for zero, possibly to avoid compile/runtime issues with zero)

    0.5 <= x < 1 → 1

📈 Positive Changes:

    6.5 <= x < 8.5 → 7

    8.5 <= x < 10.5 → 9

    10.5 <= x < 13.5 → 11

    13.5 <= x < 16.5 → 14

    16.5 <= x < 20.5 → 17

    20.5 <= x < 25.5 → 21

    25.5 <= x < 30.5 → 26

    30.5 <= x < 49.5 → 31

    49.5 <= x < 99.999 → 50

🧮 Default Case:

    Any value not falling within the defined fuzzy ranges will be rounded using round(x)

#### **Group Generalisation**: 
  📉 Negative Changes:

    -5 < x < -1 → -3

    -10 < x <= -5 → -8

    -15 < x <= -10 → -13

    -20 < x <= -15 → -18

    -25 < x <= -20 → -23

    -30 < x <= -25 → -28

    -50 < x <= -30 → -40

    -99.999 < x <= -50 → -75

⚠️ Mild Negative:

    -1 < x < 0 → -1

⚠️ Mild Positive:

    0 <= x < 1 → 1

    1 <= x < 5 → 3

📈 Positive Changes:

    5 <= x < 10 → 8

    10 <= x < 15 → 13

    15 <= x < 20 → 18

    20 <= x < 25 → 23

    25 <= x < 30 → 28

    30 <= x < 50 → 40

    50 <= x < 99.999 → 75


> Disclaimer: 
> Sometimes `Most likely next price` might conflict with `Price prediction` and this is a known bug. For example: [-3, -3, 2, 3, 4]... In this case the `Price prediction` will be UP but `Most likely next price` may be -3. In essence, always pick the `Price Direction` over the `Most likely price value` for a more accurate analysis.