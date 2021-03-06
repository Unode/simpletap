SimpleTAP
=========

``simpletap`` is a test runner that integrates with the unittest framework to
produce TAP (Test Anything Protocol) compatible output.

| ``simpletap`` has been extensively tested under Python 2.7 and is considered production ready.
| Python 3.4-3.5 is also supported but has not seen as much testing.


Usage
-----

In your test scripts, instead of:

.. code:: python

    if __name__ == "__main__":
        unittest.main()

use:

.. code:: python

    if __name__ == "__main__":
        from simpletap import TAPTestRunner
        unittest.main(testRunner=TAPTestRunner())


A small test case like:

.. code:: python

    import unittest

    class IntegerArithmeticTestCase(unittest.TestCase):
        def testAdd(self):  # test method names begin 'test*'
            "test adding values"
            self.assertEqual((1 + 2), 3)
            self.assertEqual(0 + 1, 1)

        def testMultiply(self):
            "test multiplying values"
            self.assertEqual((0 * 10), 0)
            self.assertEqual((5 * 8), 40)

        def testFail(self):
            "a failing test"
            self.assertEqual(0, 1)

        @unittest.expectedFailure
        def testExpectFail(self):
            "we saw this coming"
            self.assertEqual(0, 1)

        @unittest.skipIf(True, "Skipping this one")
        def testSkip(self):
            "pending a fix"
            self.assertEqual(0, 1)

        def testError(self):
            "oops something went wrong"
            no_such_variable + 1  # Oops!

    if __name__ == "__main__":
        from simpletap import TAPTestRunner
        unittest.main(testRunner=TAPTestRunner())

When saved in a file called ``test.py`` and executed would produce:

.. code:: TAP

    1..6
    ok 1 - test.py: test adding values
    not ok 2 - test.py: oops something went wrong
    # ERROR: NameError on file test.py line 30 in testError: 'no_such_variable + 1  # Oops!':
    #        global name 'no_such_variable' is not defined
    skip 3 - test.py: we saw this coming
    # EXPECTED_FAILURE: AssertionError on file test.py line 21 in testExpectFail: 'self.assertEqual(0, 1)':
    #                   0 != 1
    not ok 4 - test.py: a failing test
    # FAIL: AssertionError on file test.py line 16 in testFail: 'self.assertEqual(0, 1)':
    #       0 != 1
    ok 5 - test.py: test multiplying values
    skip 6 - test.py: pending a fix
    # SKIP:
    #       Skipping this one


You can also launch ``simpletap`` directly from the command line in much the same way you do with unittest:

.. code::

    python -m simpletap test.IntegerArithmeticTestCase

Deviations from standard
------------------------

The specification of Test Anything Protocol treats skipped tests as ``ok``.

During the use of this module it was found to be more useful to treat these, as
well as expected failures as extensions to the specification under the keyword ``skip``.


Testing
-------

The test suite is configured to run via `tox <http://tox.readthedocs.io/>`__.

Status:

.. image:: https://travis-ci.org/unode/simpletap.svg?branch=master
    :target: https://travis-ci.org/unode/simpletap


Projects
--------

``simpletap`` is currently being used by:

- `taskwarrior <https://github.com/taskwarrior/task/>`__
- `firefox_decrypt <https://github.com/unode/firefox_decrypt/>`__
