======
Lambda
======

Callable Objects
----------------

.. code-block:: cpp

    #include <iostream>

    class Fib {
    public:
        long operator() (const long n) {
            return (n <= 2) ? 1 : operator()(n-1) + operator()(n-2);
        }
    };

    int main() {
        Fib fib;
        std::cout << fib(10) << "\n";
        return 0;
    }

Lambda version

.. code-block:: cpp

    #include <iostream>
    #include <functional>

    int main() {
        std::function<long(long)> fib = [&](long n) {
            return (n <= 2) ? 1 : fib(n-1) + fib(n-2);
        };
        std::cout << fib(10) << "\n";
        return 0;
    }

Captureless
-----------

.. code-block:: cpp

    #include <iostream>

    int main() {
        long (*fib)(long) = [](long n) {
            long a = 0, b = 1;
            for (long i = 0; i < n; ++i) {
                long tmp = b;
                b = a + b;
                a = tmp;
            }
            return a;
        };
        std::cout << fib(10) << "\n";
        return 0;
    }

constexpr by Default
--------------------

.. code-block:: cpp

    #include <iostream>

    int main() {
        auto fib = [](long n) {
            long a = 0, b = 1;
            for (long i = 0; i < n; ++i) {
                long tmp = b;
                b = a + b;
                a = tmp;
            }
            return a;
        };

        // constexpr by default is new in c++17
        static_assert(fib(10) == 55);
        return 0;
    }

output:

.. code-block:: bash

    $ g++ -std=c++17 -g -O3 a.cpp

Generic Lambda
--------------

.. code-block:: cpp

    #include <iostream>
    #include <utility>

    class Sum {
    public:
        template <typename ...Args>
        constexpr auto operator()(Args&& ...args) {
            return (std::forward<Args>(args) + ...);
        }
    };

    int main() {
        Sum sum;
        constexpr int ret = sum(1,2,3,4,5);
        std::cout << ret << std::endl;
        return 0;
    }

The snippet is equal to the following example

.. code-block:: cpp

    #include <iostream>
    #include <utility>

    int main() {
        auto sum = [](auto&& ...args) {
            return (std::forward<decltype(args)>(args) + ...);
        };
        constexpr int ret = sum(1,2,3,4,5);
        std::cout << ret << std::endl;
        return 0;
    }

Reference
---------

1. `Back to Basics: Lambdas from Scratch`_

.. _Back to Basics\: Lambdas from Scratch: https://youtu.be/3jCOwajNch0