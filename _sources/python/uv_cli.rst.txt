==================
Modern Python CLI
==================

.. note::

   This tutorial is designed for **data scientists** who usually run scripts with::

      python script.py

   We will show you how to transform such scripts into a **real Command-Line Interface (CLI)**.
   First, we start with a very simple library called ``click``.
   Then, we move on to a modern, professional stack with ``Typer``, ``Rich``, ``Questionary``, and ``Pyfiglet``.

source:

-----------------------------------
Part 1: A First CLI with ``click``
-----------------------------------

Initiating a uv project
=======================

.. code-block:: python

    uv init --package tuto_cli

Installing ``click``
=====================

Install the library with::

   uv add click

Creating the File
=================

Create a file called ``src/tuto_cli/cli_basic.py`` in your project folder:

.. code-block:: python

   import click

    @click.group()
    def main():
        pass

    @main.command()
    @click.argument("price", type=float)
    @click.argument("quantity", type=int)
    @click.option("--taxes", default=0.0, type=float, help="Tax percentage")
    def total(price, quantity, taxes):
        """Compute the total cost with optional taxes"""
        subtotal = price * quantity
        total_value = subtotal * (1 + taxes / 100)
        print(f"💰 Price: {price} € x {quantity} = {subtotal:.2f} €")
        if taxes > 0:
            print(f"➕ Taxes: {taxes}% → Total = {total_value:.2f} €")

    if __name__ == "__main__":
        main()


Running the CLI
===============

Execute the script with::

   uv run src/tuto_cli/cli_basic.py total 10 3 --taxes 20

Output::

   💰 Price: 10.0 € x 3 = 30.00 €
   ➕ Taxes: 20.0% → Total = 36.00 €

Limitations of ``click``
========================

- **Minimal features**: no colors, no tables, no progress bars.
- **No interactivity**: you cannot ask the user for confirmation or choices.
- **Low maintenance**: ``cli`` is not widely used or updated.
- **Poor documentation** compared to modern frameworks.

This is why we now move on to a **modern stack**.

--------------------------------------
Part 2: A Modern CLI with ``Typer``
--------------------------------------

Installing Dependencies
=======================

Add the following libraries::

   uv add typer[all] rich questionary pyfiglet

Writing the CLI
===============

Open ``src/tuto_cli/cli.py`` :

.. code-block:: python

    import typer
    import pyfiglet
    import questionary
    from rich.console import Console

    app = typer.Typer()
    console = Console()

    @app.command()
    def total(
        price: float = typer.Option(..., "--price", "-p", help="Unit price in euros"),
        quantity: int = typer.Option(..., "--quantity", "-q", help="Number of items"),
        taxes: float = typer.Option(0.0, "--taxes", "-t", help="Tax rate (%)"),
    ):
        """
        Compute the total cost of items, optionally with taxes.
        """

        # Pyfiglet banner
        banner = pyfiglet.figlet_format("Total CLI")
        console.print(f"[bold cyan]{banner}[/bold cyan]")

        # Confirmation with Questionary
        confirm = questionary.confirm(
            f"Do you want to calculate {quantity} item(s) at {price}€ with {taxes}% taxes?"
        ).ask()

        if confirm:
            subtotal = price * quantity
            total_value = subtotal * (1 + taxes / 100)
            console.print(f"💰 [green]Subtotal[/green]: {subtotal:.2f} €")
            if taxes > 0:
                console.print(
                    f"➕ [blue]Taxes ({taxes}%)[/blue] → "
                    f"[bold yellow]{total_value:.2f} €[/bold yellow]"
                )
            else:
                console.print(f"✅ [bold yellow]Total[/bold yellow]: {total_value:.2f} €")
        else:
            console.print("[red]Operation cancelled.[/red]")

    if __name__ == "__main__":
        app()


Running the CLI
===============

Run with::

   uv run src/tuto_cli/cli.py --price 10 --quantity 3 --taxes 20

Example Output::

   _______          _        _____ _      _____
  |__   __|        | |      / ____| |    |_   _|
     | | ___   ___ | | __  | |    | |      | |
     | |/ _ \ / _ \| |/ /  | |    | |      | |
     | | (_) | (_) |   <   | |____| |____ _| |_
     |_|\___/ \___/|_|\_\   \_____|______|_____|

   ? Do you want to calculate 3 item(s) at 10.0€ with 20.0% taxes? Yes
   💰 Subtotal: 30.00 €
   ➕ Taxes (20.0%) → 36.00 €

Exploring Help
==============

Typer auto-generates documentation::

   uv run src/tuto_cli/cli.py --help

Output::

    Usage: cli.py [OPTIONS]

    Compute the total cost of items, optionally with taxes.

    ╭─ Options ────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
    │ *  --price               -p      FLOAT    Unit price in euros [required]                                             │
    │ *  --quantity            -q      INTEGER  Number of items [required]                                                 │
    │    --taxes               -t      FLOAT    Tax rate (%) [default: 0.0]                                                │
    │    --install-completion                   Install completion for the current shell.                                  │
    │    --show-completion                      Show completion for the current shell, to copy it or customize the         │
    │                                           installation.                                                              │
    │    --help                                 Show this message and exit.                                                │
    ╰──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯


-----------------------------------
Benchmark: ``click`` vs ``Typer``
-----------------------------------

Here is a comparison between the two approaches:

.. list-table::
   :header-rows: 1
   :widths: 20 20 30

   * - Feature
     - ``click`` (basic)
     - ``Typer + Rich stack``
   * - Maintenance
     - Low, rarely updated
     - Active, large community
   * - Arguments & options
     - Supported
     - Supported + full help
   * - Colors & formatting
     - No
     - Yes (via Rich)
   * - Interactivity
     - No
     - Yes (via Questionary)
   * - ASCII art banners
     - No
     - Yes (via Pyfiglet)
   * - Help & documentation
     - Minimal
     - Auto-generated, detailed
   * - Use cases
     - Quick demos
     - Professional-grade tools


Conclusion
==========

- ``click`` is fine for a **very quick experiment**, but limited.
- ``Typer + Rich + Questionary + Pyfiglet`` turns your script into a **professional tool**.
- For data scientists, this means you can **share cleaner, friendlier tools** with colleagues, automate pipelines, and create impressive demos.

Next Steps
==========

- Add more commands (e.g. ``discount`` or ``convert``).
- Connect the CLI to your **data pipeline** or **machine learning scripts**.
- Share it as a Python package with your team.

With these steps, you just learned how to **level up your Python scripts** into real, user-friendly CLIs. 🎉
