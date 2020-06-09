Tutorial 2: Using pyMOR’s discretization toolkit
================================================

.. code-links::
    :timeout: -1


One of pyMOR's main features is easy integration of external Solvers. In this tutorial
we will do this step-by-step for a toy solver of the diffusion equation written in C++.
First we need a class to store our data in with some basic linear algebra operations
decared on it.

.. code-block:: cpp

  include <vector>

  class Vector {
    friend class DiffusionOperator;
  public:
    Vector(int dim, double value);
    Vector(const Vector& other);
    const int dim;
    void scal(double val);
    void axpy(double a, const Vector& x);
    double dot(const Vector& other) const;
    double* data();
  private:
    std::vector<double> _data;
  };

Next we need the operator discretizes our PDE

.. code-block:: cpp

  class DiffusionOperator {
  public:
    DiffusionOperator(int n, double left, double right);
    const int dim_source;
    const int dim_range;
    void apply(const Vector& U, Vector& R) const;
  private:
    const double h;
    const double left;
    const double right;
  };

on the domain :math:`\Omega:= (left, right) \subset \mathbb{R}`.

Together with some header guards these two snippets make up our :download:`model.hh <minimal_cpp_demo/model.hh>`.

The definitions for the Vector class are pretty straight forward:

.. code-block:: cpp

  Vector::Vector(int dim, double value) : _data(dim, value), dim(dim) {}

  Vector::Vector(const Vector& other) : _data(other._data), dim(other.dim) {}

  void Vector::scal(double val) {
    for (int i = 0; i < dim; i++) {
      _data[i] *= val;
    }
  }

  void Vector::axpy(double a, const Vector& x) {
    assert(x.dim == dim);
    for (int i = 0; i < dim; i++) {
      _data[i] += a * x._data[i];
    }
  }

  double Vector::dot(const Vector& other) const {
    assert(other.dim == dim);
    double result = 0;
    for (int i = 0; i < dim; i++) {
      result += _data[i] * other._data[i];
    }
    return result;
  }

  double* Vector::data() {
    return _data.data();
  }


:download:`model.cc <minimal_cpp_demo/model.cc>`
:download:`CMakeLists.txt <minimal_cpp_demo/CMakeLists.txt>`


.. nbplot::

   %%bash
   ls -la
   mkdir minimal_cpp_demo/build
   cd minimal_cpp_demo/build
   cmake ..
   make
