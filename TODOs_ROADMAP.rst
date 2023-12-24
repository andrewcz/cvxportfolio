TODOs and ROADMAP
=================

Cvxportfolio follows the `semantic versioning <https://semver.org>`_
specification for changes in its public API. The public API is defined
as:

- public methods, *i.e.*, without a leading underscore ``_``
- methods and objects clearly documented as such and/or used in the examples.

*Internal* methods that are used by 
Cvxportfolio objects to communicate with each other (or other tasks) and don't
have a leading underscore, are considered public if they are exposed through 
the HTML documentation and/or are used in the examples.

In this document we list the planned
changes, in particular the ones that are relevant for semantic versioning, and 
their planned release.

``cvxportfolio.cache``
----------------------

- [ ] Not part of public API; to be redesigned and probably dropped. Should use
  ``_loader_*`` and ``_storer_*`` from ``cvxportfolio.data``. Target ``1.1.0``.

``cvxportfolio.forecast``
-------------------------

- cache logic needs improvement, not easily exposable to third-parties now with ``dataclass.__hash__``

  - drop decorator
  - drop dataclass
  - cache IO logic should be managed by forecaster not by simulator, could be done by ``initialize_estimator``; maybe enough to just
    define it in the base class of forecasters
- improve names of internal methods, clean them (lots of stuff can be re-used at universe change, ...)
- generalize the mean estimator:

  - use same code for ``past_returns``, ``past_returns**2``, ``past_volumes``, ...
  - add rolling window option, should be in ``pd.Timedelta``
  - add exponential moving avg, should be in half-life ``pd.Timedelta``
- add same extras to the covariance estimator
- goal: make this module crystal clear; third-party ML models should use it (at least for caching)

``cvxportfolio.estimator``
--------------------------

- [ ] ``DataEstimator`` needs refactoring, too long and complex methods. Target 
  ``1.0.4``. 
- ``Estimator`` could define base logic for on-disk caching. By itself it
  wouldn't do anything, actual functionality implemented by forecasters' base
  class.

  - [ ] ``initialize_estimator`` could get optional market data partial
    signature for caching. Default None, no incompatible change.
  - [ ] Could get a ``finalize_estimator`` method used for storing
    data, like risk models on disk, doesn't need arguments; it can use the
    partial signature got above. No incompatible change.

``cvxportfolio.data``
--------------------------

``cvxportfolio.simulator``
--------------------------

``cvxportfolio.risks``
----------------------

``cvxportfolio.hyperparameters``
-------------------------

- [ ] Clean up interface w/ ``MarketSimulator``, right now it calls private 
  methods, maybe enough to make them public. Target ``1.0.4``.
- [ ] Add risk/fine default ``GammaTrade``, ``GammaRisk`` (which are
  ``RangeHyperParameter``) modeled after original examples from paper. 
  Target ``1.1.0``.

``cvxportfolio.policies``
-------------------------

- [ ] Add `AllIn` policy, which allocates all to a single name (like 
  ``AllCash``). Target ``1.1.0``.

Optimization policies
~~~~~~~~~~~~~~~~~~~~~

- [ ] Improve behavior for infeasibility/unboundedness/solver error. Target 
  ``1.1.0``.
- [ ] Improve ``__repr__`` method, now hard to read. Target ``1.0.4``.

``cvxportfolio.constraints``
----------------------------

- [ ] Add missing constraints from the paper. Target ``1.1.0``.
- [ ] Make ``MarketNeutral`` accept arbitrary benchmark (policy object).

``cvxportfolio.result``
-----------------------

- [ ] Make ``BackTestResult`` interface methods with ``MarketSimulator`` 
  public. 
- [ ] Add a ``backruptcy`` property (boolean). Amend ``sharpe_ratio``
  and other aggregate statistics (as best as possible) to return ``-np.inf``
  if back-test ended in backruptcy. This is needed specifically for
  hyper-parameter optimization. Target ``1.0.4``.


Development & testing
---------------------

- [ ] Add extra pylint checkers. 
  
  - [ ] Code complexity. Target ``1.0.4``. 
- [ ] Consider removing downloaded data from ``test_simulator.py``,
  so only ``test_data.py`` requires internet. 

Documentation
-------------

- [ ] Improve examples section, also how "Hello world" is mentioned in readme.
  Target ``1.0.4``, PR #118.
- [ ] Manual.
- [ ] Quickstart, probably to merge into manual.

Examples
--------

- [ ] Restore examples from paper. Target ``1.0.4``, PR #118.
- [ ] Expose more (all?) examples through HTML docs. Target ``1.0.4``, PR #118.
- [ ] Consider making examples a package that can be pip installed.