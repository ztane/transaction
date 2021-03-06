Changes
=======

- Add support for savepoint.release(). Some databases only support a limited
  number of savepoints or subtransactions, this provides an opportunity for a
  data manager to free those resources.

1.4.2 (unreleased)
------------------

- Nothing changed yet.


1.4.1 (2013-02-20)
------------------

- Document that values returned by ``sortKey`` must be strings, in order
  to guarantee total ordering.

- Fix occasional RuntimeError: dictionary changed size during iteration errors
  in transaction.weakset on Python 3.

1.4.0 (2013-01-03)
------------------

- Updated Trove classifiers.

1.4.0b1 (2012-12-18)
--------------------

- Converted existing doctests into Sphinx documentation (snippets are
  exercised via 'tox').

- 100% unit test coverage.

- Backward incompatibility:   raise ValueError rather than AssertionError
  for runtime errors:

  - In ``Transaction.doom`` if the transaction is in a non-doomable state.

  - In ``TransactionManager.attempts`` if passed a non-positive value.

  - In ``TransactionManager.free`` if passed a foreign transaction.

- Declared support for Python 3.3 in ``setup.py``, and added ``tox`` testing.

- When a non-retryable exception was raised as the result of a call to
  ``transaction.manager.commit`` within the "attempts" machinery, the
  exception was not reraised properly.  Symptom: an unrecoverable exception
  such as ``Unsupported: Storing blobs in <somestorage> is not supported.``
  would be swallowed inappropriately.

1.3.0 (2012-05-16)
------------------

- Added Sphinx API docuementation.

- Added explicit support for PyPy.

- Dropped use of Python3-impatible ``zope.interface.implements`` class
  advisor in favor of ``zope.interface.implementer`` class decorator.

- Added support for continuous integration using ``tox`` and ``jenkins``.

- Added ``setup.py docs`` alias (installs ``Sphinx`` and dependencies).

- Added ``setup.py dev`` alias (runs ``setup.py develop`` plus installs
  ``nose`` and ``coverage``).

- Python 3.3 compatibility.

- Fix "for attempt in transaction.attempts(x)" machinery, which would not
  retry a transaction if its implicit call to ``.commit()`` itself raised a
  transient error.  Symptom: seeing conflict errors even though you thought
  you were retrying some number of times via the "attempts" machinery (the
  first attempt to generate an exception during commit would cause that
  exception to be raised).

1.2.0 (2011-12-05)
------------------

New Features:

- Python 3.2 compatibility.

- Dropped Python 2.4 and 2.5 compatibility (use 1.1.1 if you need to use
  "transaction" under these Python versions).

1.1.1 (2010-09-16)
------------------

Bug Fixes:

- Code in ``_transaction.py`` held on to local references to traceback
  objects after calling ``sys.exc_info()`` to get one, causing
  potential reference leakages.

- Fixed ``hexlify`` NameError in ``transaction._transaction.oid_repr``
  and add test.

1.1.0 (1010-05-12)
------------------

New Features:

- Transaction managers and the transaction module can be used with the
  with statement to define transaction boundaries, as in::

     with transaction:
         ... do some things ...

  See transaction/tests/convenience.txt for more details.

- There is a new iterator function that automates dealing with
  transient errors (such as ZODB confict errors). For example, in::

     for attempt in transaction.attempts(5):
         with attempt:
             ... do some things ..

  If the work being done raises transient errors, the transaction will
  be retried up to 5 times.

  See transaction/tests/convenience.txt for more details.

Bugs fixed:

- Fixed a bug that caused extra commit calls to be made on data
  managers under certain special circumstances.

  https://mail.zope.org/pipermail/zodb-dev/2010-May/013329.html

- When threads were reused, transaction data could leak accross them,
  causing subtle application bugs.

  https://bugs.launchpad.net/zodb/+bug/239086

1.0.1 (2010-05-07)
------------------

- LP #142464:  remove double newline between log entries:  it makes doing
  smarter formatting harder.

- Updated tests to remove use of deprecated ``zope.testing.doctest``.

1.0.0 (2009-07-24)
------------------

- Fix test that incorrectly relied on the order of a list that was generated
  from a dict.

- Remove crufty DEPENDENCIES.cfg left over from zpkg.

1.0a1 (2007-12-18)
------------------

= Initial release, branched from ZODB trunk on 2007-11-08 (aka
  "3.9.0dev").

- Remove (deprecated) support for beforeCommitHook alias to
  addBeforeCommitHook.

- Add weakset tests.

- Remove unit tests that depend on ZODB.tests.utils from
  test_transaction (these are actually integration tests).
