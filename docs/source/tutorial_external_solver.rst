Tutorial 2: Using pyMOR’s discretization toolkit
================================================

One of pyMOR's main features is easy integration of external Solvers. In this tutorial
we will do this step-by-step for a toy solver of the diffusion equation written in C++.
First we need a class to store our data in with some basic linear algebra operations
declared on it.


.. literalinclude:: minimal_cpp_demo/model.hh
    :lines: 4-19
    :language: cpp


Next we need the operator discretizes our PDE

.. literalinclude:: minimal_cpp_demo/model.hh
    :lines: 22-32
    :language: cpp

on the domain :math:`\Omega:= (left, right) \subset \mathbb{R}`.

Together with some header guards these two snippets make up our :download:`model.hh <minimal_cpp_demo/model.hh>`.

The definitions for the Vector class are pretty straight forward:

.. literalinclude:: minimal_cpp_demo/model.cc
    :lines: 7-35
    :language: cpp

Just like the diffusion operator that computes a central differenes stencil:

.. literalinclude:: minimal_cpp_demo/model.cc
    :lines: 39-49
    :language: cpp

.. jupyter-kernel:: bash
    :id: make

.. jupyter-execute::

   mkdir -p source/minimal_cpp_demo/build
   cd source/minimal_cpp_demo/build
   cmake ..
   make

FILL TEXT

.. jupyter-kernel::
.. jupyter-execute::

  import sys
  sys.path.insert(0, 'source/minimal_cpp_demo/build')

  import model
  dir(model)


Download the notebook for first kernel: :jupyter-download:notebook:`make`
            notebook and script for second kernel :jupyter-download:notebook:`tutorial_external_solver`
            :jupyter-download:script:`tutorial_external_solver`

:download:`model.cc <minimal_cpp_demo/model.cc>`
:download:`CMakeLists.txt <minimal_cpp_demo/CMakeLists.txt>`
