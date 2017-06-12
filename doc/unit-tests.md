Compiling/running unit tests
------------------------------------

Unit tests will be automatically compiled if dependencies were met in `./configure`
and tests weren't explicitly disabled.

After configuring, they can be run with `make check`.

To run the g0coind tests manually, launch `src/test/test_g0coin`.

To add more g0coind tests, add `BOOST_AUTO_TEST_CASE` functions to the existing
.cpp files in the `test/` directory or add new .cpp files that
implement new BOOST_AUTO_TEST_SUITE sections.

To run the g0coin-qt tests manually, launch `src/qt/test/test_g0coin-qt`

To add more g0coin-qt tests, add them to the `src/qt/test/` directory and
the `src/qt/test/test_main.cpp` file.
