Numba
=====

:Version: 2025-10

Everyone knows: Python is slow — especially when it comes to loops.

Instead of vectorizing *everything* or rewriting your logic in C/C++, there’s a hidden gem that deserves more attention: **Numba**.

What is Numba?
---------------

Introduction
^^^^^^^^^^^^

Numba is a **Just-In-Time (JIT)** compiler for Python, powered by LLVM. It takes your regular Python functions and compiles them into native machine code.

**Result?** Performance close to C, without leaving Python.

Extension in pandas
^^^^^^^^^^^^^^^^^^^

The `apply` method in Pandas is typically something we try to avoid,
as it often slows down code execution significantly.

But sometimes, we simply have no other choice.

So, how can we speed up `apply` without installing any extra libraries?

Since version 2.2, it's possible to run the function using **Numba** instead of plain Python, thanks to the `engine` parameter.

Numba enables faster computation by compiling the code using the **Just-In-Time (JIT)** method to optimize performance.

Moreover, operations can now be **parallelized**, further boosting execution speed.


Example: Compute the Sum of Squares
-----------------------------------

**Without Numba (slow):**

.. code-block:: python

    def sum_of_squares(arr):
        total = 0
        for x in arr:
            total += x * x
        return total

**With Numba (fast):**

.. code-block:: python

    from numba import njit

    @njit
    def sum_of_squares(arr):
        total = 0
        for x in arr:
            total += x * x
        return total

Performance Comparison
-----------------------

With an array of 10 million elements:

- Pure Python: ~1.3 seconds
- With Numba: **~0.02 seconds**

Why It’s Great
--------------

- You keep your code **clean and native**
- You compile **only the critical parts**
- Perfect for **custom algorithms** and **complex loops**

Takeaway
--------

If you're looking to **optimize** your code without breaking it or switching languages,
add **Numba** to your data science toolbox.
