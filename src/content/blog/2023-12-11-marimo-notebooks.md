---
title: Exploring Alternatives to Jupyter Notebooks - Introducing Marimo Notebooks
pubDatetime: 2023-12-11T23:00:00Z
postSlug: marimo-notebooks
featured: true
draft: false
tags:
  - data-science
  - tools
  - notebooks
  - interactive-python
description: The next generation python notebooks where every program is also an interactive web-app
---

> Ever since its inception in 2014, Jupyter Notebooks have been the go-to for data enthusiasts, scientists, and educators alike. Their interactive interface and multi-language support have made them indispensable. But let’s face it, even the best tools have their quirks and room for improvement. Despite being a Jupyter aficionado, I believe it’s essential to periodically scrutinize alternatives to ensure we’re using the best tools available. Enter Marimo, a reactive Python notebook promising to address Jupyter’s shortcomings. Let’s dive into why you might want to consider it.

### Documented Limitations of Jupyter

Alright, folks, let’s address the elephant in the room – Jupyter’s quirks and quibbles. While Jupyter Notebooks have revolutionized data science, they aren’t without their flaws. Here are some of the most documented gripes:

1. **Hidden State Issues**: The order of cell execution can lead to inconsistencies and bugs. Running cells out of order? Good luck keeping track of that.
2. **Version Control Nightmares:** Storing notebooks as JSON files is a version control nightmare. Diffing and merging? Not so fun.
3. **Manual Updates Required:** Changes in one part of a notebook don’t propagate automatically. Manually re-executing dependent cells is a recipe for errors.
4. **Performance and Scalability:** Jupyter can lag with large datasets or complex computations. Plus, running a server can be a hassle in resource-constrained environments.
5. **Interactivity Limitations:** Widgets and extensions are great, but setting them up can be a pain. Not always the smoothest experience.

These are just a few of the hiccups that have users occasionally pulling their hair out. And this is where Marimo steps in, promising to smooth out these rough edges.

### How Marimo is the Solution

Marimo swoops in like a caped hero to rescue us from Jupyter’s pitfalls with its arsenal of next-gen features:

1. **Reactive Programming:** Marimo’s reactive model ensures that changes to inputs automatically update all dependent outputs. Say goodbye to manual cell re-executions and hello to seamless updates.
2. **Python File Storage:** Storing notebooks as .py files makes version control a breeze. Git merges and diffs are finally your friends again.
3. **No Hidden State:** Marimo ensures a visible, reproducible state, making debugging straightforward and reducing those pesky hidden state issues.
4. **Executable as Scripts:** Marimo notebooks double as Python scripts. This means you can run them anywhere, integrating smoothly into broader projects without extra hoops.
5. **Deployable as Web Apps:** Transform your notebooks into interactive web apps effortlessly. Share interactive data visualizations and dashboards without forcing anyone to install additional software.
6. **WebAssembly (WASM) Compatibility:** Marimo can run entirely in your browser using WASM, making it accessible on platforms where installing Python might be challenging.
7. **Enhanced Collaboration:** Real-time collaboration is natively integrated. Multiple users can work on the same notebook simultaneously, making teamwork smoother than ever.

By addressing these limitations, Marimo positions itself as a robust alternative to Jupyter, bringing in performance, collaboration, and usability improvements that make it a worthy contender in the realm of interactive notebooks.

### Getting Started with Marimo

Ready to dive into Marimo? Let’s get you up and running with this exciting new tool. Follow these steps to start exploring Marimo’s capabilities:

#### Installation:

First, make sure you have Python installed. Then, install Marimo using pip:

```bash
pip install marimo plotly pandas
```

#### Launching Marimo:

Once installed, launch Marimo by running:

```bash
marimo edit
```

This command starts the Marimo server and opens the interface in your default web browser.

#### Example Code:

Here’s a simple data analysis task to get you started, let's save this file as `analysis.py`:

```python
import marimo


# Initialize the CSV data as a multiline string
csv_data = """#,Name,Type 1,Type 2,Total,HP,Attack,Defense,Sp. Atk,Sp. Def,Speed,Generation,Legendary
1,Bulbasaur,Grass,Poison,318,45,49,49,65,65,45,1,False
2,Ivysaur,Grass,Poison,405,60,62,63,80,80,60,1,False
3,Venusaur,Grass,Poison,525,80,82,83,100,100,80,1,False
3,VenusaurMega Venusaur,Grass,Poison,625,80,100,123,122,120,80,1,False
4,Charmander,Fire,,309,39,52,43,60,50,65,1,False
5,Charmeleon,Fire,,405,58,64,58,80,65,80,1,False
6,Charizard,Fire,Flying,534,78,84,78,109,85,100,1,False
6,CharizardMega Charizard X,Fire,Dragon,634,78,130,111,130,85,100,1,False
6,CharizardMega Charizard Y,Fire,Flying,634,78,104,78,159,115,100,1,False
7,Squirtle,Water,,314,44,48,65,50,64,43,1,False
8,Wartortle,Water,,405,59,63,80,65,80,58,1,False
9,Blastoise,Water,,530,79,83,100,85,105,78,1,False
9,BlastoiseMega Blastoise,Water,,630,79,103,120,135,115,78,1,False
10,Caterpie,Bug,,195,45,30,35,20,20,45,1,False
11,Metapod,Bug,,205,50,20,55,25,25,30,1,False
12,Butterfree,Bug,Flying,395,60,45,50,90,80,70,1,False
13,Weedle,Bug,Poison,195,40,35,30,20,20,50,1,False
14,Kakuna,Bug,Poison,205,45,25,50,25,25,35,1,False
15,Beedrill,Bug,Poison,395,65,90,40,45,80,75,1,False
15,BeedrillMega Beedrill,Bug,Poison,495,65,150,40,15,80,145,1,False
16,Pidgey,Normal,Flying,251,40,45,40,35,35,56,1,False
17,Pidgeotto,Normal,Flying,349,63,60,55,50,50,71,1,False
18,Pidgeot,Normal,Flying,479,83,80,75,70,70,101,1,False
18,PidgeotMega Pidgeot,Normal,Flying,579,83,80,80,135,80,121,1,False
19,Rattata,Normal,,253,30,56,35,25,35,72,1,False
20,Raticate,Normal,,413,55,81,60,50,70,97,1,False"""

# Create the Marimo app
app = marimo.App(width="full")


@app.cell
def __(mo, plot):
    mo.hstack([plot, plot.value])
    return

@app.cell
def __():
    import marimo as mo
    import pandas as pd
    import plotly.express as px
    from io import StringIO

    return mo, pd, px, StringIO


@app.cell
def __(pd, mo, StringIO):
    # Read the CSV data into a DataFrame
    data = pd.read_csv(StringIO(csv_data))
    poke_type = mo.ui.dropdown(
        label="Select Type", options=["All"] + data["Type 1"].unique().tolist(), value="All"
    )
    return poke_type, data


@app.cell
def __(mo, px, poke_type, data):
    if poke_type == "All":
        filtered_data = data
    else:
        filtered_data = data[data["Type"] == poke_type]

    fig = px.scatter(
        filtered_data,
        x="Attack",
        y="Defense",
        color="Name",
        size="HP",
        hover_name="Name",
        title=f"{poke_type} Type Pokémon: Attack vs Defense",
    )
    _plot = px.scatter(filtered_data, x="Horsepower", y="Miles_per_Gallon", color="Origin")
    plot = mo.ui.plotly(_plot)
    return plot


@app.cell
def __():
    return


if __name__ == "__main__":
    app.run()
```

### Explanation:

- **CSV Data:** We initialize Pokémon data as a multiline string and load it into a pandas DataFrame.
- **Marimo App:** We create a Marimo app instance with `app = marimo.App(width="full")`.
- **UI Elements:** We add a dropdown menu to select the Pokémon type.
- **Reactive Plot Function:** The `update_plot` function creates a scatter plot of Attack vs. Defense, color-coded by Pokémon name and sized by HP. This function is wrapped in a `@app.cell` decorator to make it reactive.
- **Display and Run:** We display the dropdown and plot using `mo.hstack()` and start the app with `app.run()`.

### Running the Notebook:

1. Save the code in a file named `pokemon_plot.py`.
2. Open a terminal and run the app using:
   ```bash
   marimo run pokemon_plot.py
   ```
3. The app will start and open in your default web browser, where you can interactively select Pokémon types and see the plot update in real time.

### Conclusion:

Deploying Marimo notebooks as web apps is not just easy, it's downright magical. With just a few commands, your interactive notebooks become shareable, interactive web apps, perfect for collaboration and presentation. So go ahead, turn those data insights into something everyone can see and interact with.

#### Verdict (as of July 9, 2024):

While the idea of reactive, interactive notebooks that can be deployed as scripts, notebooks, or apps is still a noble one, the Marimo project has its fair share of quirks and bumps in the road. Think of it as a charmingly eccentric inventor’s workshop – full of potential but a bit chaotic.

As of today, getting even a simple example to run without hiccups is like trying to catch a rare Pokémon with a basic Poké Ball – frustratingly elusive. It’s curious that in 2024, with daily active contributions, there isn’t a “Hello World” that just works out of the box. Despite my best efforts, I can’t find any mention of the “kernel not found” error that I seem to be getting right after a simple install (pip).

But let’s not lose hope just yet! Maybe the maintainers are gearing up for a grand comeback. Perhaps they’re planning to sprinkle some magic dust and bring Marimo to life in ways we’ve only dreamed of. Until then, we wait with a mix of anticipation and amusement, hoping that soon we’ll see Marimo living up to its full potential. Stay tuned, keep those fingers crossed, and enjoy the ride!

### References:

Explore more about Marimo and its features from these resources:

- [marimo: a reactive Python notebook that runs in the browser](https://blog.pyodide.org/posts/marimo/)
- [[P] I built marimo — an open-source reactive Python notebook that’s stored as a .py file, executable as a script, and deployable as an app.](https://www.reddit.com/r/MachineLearning/comments/191rdwq/p_i_built_marimo_an_opensource_reactive_python/)
- [Can Marimo replace Jupyter notebooks?](https://medium.com/@flyingjony/can-marimo-replace-jupyter-notebooks-fb8c7210ad35)
- [made with marimo](https://marimo.io/use-cases)
