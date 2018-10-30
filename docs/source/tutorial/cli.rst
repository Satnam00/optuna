.. _cli:

Command-Line Interface
======================

Let us assume you are not in IPython shell and writing a Python script files instead.
It is totally fine to write scripts like the following:

.. code-block:: python

    import optuna


    def objective(trial):
        x = trial.suggest_uniform('x', -10, 10)
        return (x - 2) ** 2


    if __name__ == '__main__':
        study = optuna.create_study()
        study.optimize(objective, n_trials=100)
        print('Best value: {} (params: {})\n'.format(study.best_value, study.best_params))

However, we can reduce boilerplate codes by using our ``optuna`` command.
Let us assume that ``hoge.py`` contains only the following code.

.. code-block:: python

    def objective(trial):
        x = trial.suggest_uniform('x', -10, 10)
        return (x - 2) ** 2

Even so, we can invoke the optimization as follows. (Don't care about ``--storage sqlite:///example.db`` for now, which is described in :ref:`rdb`.)

.. code-block:: bash

    $ cat hoge.py
    def objective(trial):
        x = trial.suggest_uniform('x', -10, 10)
        return (x - 2) ** 2

    $ STUDY_NAME=`optuna create-study --storage sqlite:///example.db`
    $ optuna study optimize hoge.py objective --n-trials=100 --storage sqlite:///example.db --study $STUDY_NAME
    [I 2018-05-09 10:40:25,196] Finished a trial resulted in value: 54.353767789264026. Current best value is 54.353767789264026 with parameters: {'x': -5.372500782588228}.
    [I 2018-05-09 10:40:25,197] Finished a trial resulted in value: 15.784266965526376. Current best value is 15.784266965526376 with parameters: {'x': 5.972941852774387}.
    ...
    [I 2018-05-09 10:40:26,204] Finished a trial resulted in value: 14.704254135013741. Current best value is 2.280758099793617e-06 with parameters: {'x': 1.9984897821018828}.

Please note that ``hoge.py`` only contains the definition of the objective function.
By giving the script file name and the method name of objective function to ``optuna study optimize`` command,
we can invoke the optimization.
