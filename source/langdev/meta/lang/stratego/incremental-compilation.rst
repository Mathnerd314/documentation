Incremental Compilation for Stratego
------------------------------------

.. note :: This feature will appear in the next version of Spoofax (2.6.0). It is currently only available in the nightly build. 

.. warning :: This feature is still experimental, please open issues when your build fails or your program has different behaviour under this build setting.

The Stratego compiler is usually the slowest part in the build of a Spoofax project. To improve on the development experience, we have added an incremental compilation option for Stratego. This can be opted into by editing the ``metaborg.yaml`` file:

.. code:: yaml

  language:
    stratego:
      build: incremental
      format: jar

Your file most likely said nothing of the ``build``, meaning it was on the ``batch`` setting. The format was probably on ``ctree``. If that is the case you will also need to find the ``provider`` setting in your ESV files, likely in ``editor/Main.esv``. The provider settings should be as follows:

.. code:: esv

	provider: target/metaborg/stratego.jar
	provider: target/metaborg/stratego-javastrat.jar

Note that a clean build using this setting is necessary at first. It will likely take significantly longer than a clean build using the ``ctree`` format. All subsequent builds should be faster. 

Limitations
~~~~~~~~~~~

The incremental compilation scheme does not do any static checking, so it will compile modules that refer to non-existing strategies. The result is broken Java code, which may still be compiled by the Eclipse Java Compiler. Keep an eye on the `src-gen/stratego-java` directory in your project explorer. If there are error markers on it, something went wrong.

This incremental compilation implementation does not support overlays. Overlays are likely to become deprecated in Stratego. 

Certain edge-cases with higher-order strategies are not supported. In particular, passing a higher-order strategy from the standard library to another strategy is not supported (e.g. ``foo(map)`` where ``foo(s) = s(bar)``). 