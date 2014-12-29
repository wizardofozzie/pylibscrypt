Scrypt for Python
==

There are a lot of different scrypt modules for Python, but none of them have
everything that I'd like, so here's One More[1].


Features
--
* Uses system libscrypt[2] as the first choice.
* If that isn't available, tries the scrypt Python module[3] or libsodium[4].
* Offers a pure Python scrypt implementation for when there's no C scrypt.
* Not unusably slow, even in pure Python... at least with pypy[5].

With PyPy as the interpreter the Python implementation is around one fifth the
speed of C scrypt. With CPython it is between about 50x and 250x slower.


Requirements
--
* Python 2.7 or 3.4 or so. Pypy 2.2 also works. Older versions may or may not.
* If you want speed, you should use one of:
  - libscrypt 1.8+ (older may work)
  - py-scrypt 0.6+ (pip install scrypt)
  - libsodium 0.6+


Usage
--

You can install the most recent release from PyPi using:

    pip install pylibscrypt

You most likely want to create MCF hashes and store them somewhere, then check
user-entered passwords against those hashes. For that you only need to use two
functions from the API:

    from pylibscrypt import *
    # Generate an MCF hash with random salt
    mcf = scrypt_mcf('Hello World')
    # Test it
    print(scrypt_mcf_check(mcf, 'Hello World'))   # prints True
    print(scrypt_mcf_check(mcf, 'HelloPyWorld'))  # prints False

For full API, you can try help(pylibscrypt) from python after importing.

It is highly recommended that you use a random salt, i.e. don't pass one.

Additionally, using the $7$ MCF format by calling with prefix=$7$ is recommended
because it may be made the default in a future release.


Versioning
--
The package has a version number that can be read from python like so:

    print(pylibscrypt.__version__)

The version number is of the form X.Y.Z, following Semantic Versioning[6].
Releases are tagged vX.Y.Z and release branches bX.Y.x when they differ from
master.


Development
--
Development happens on GitHub[2]. If you find a bug, please open an issue there.

Running pylibscrypt.tests will test all implementations with some quick tests.
Running any implementation directly (e.g. pylibscrypt.pylibsodium) will also
compare to scrypt test vectors from the paper but this is slow for the pure
Python version unless you have pypy.

Running pylibscrypt.fuzz will do some basic fuzz tests with semi-randomly chosen
inputs.

[1]:https://xkcd.com/927/
[2]:https://github.com/technion/libscrypt
[3]:https://bitbucket.org/mhallin/py-scrypt/src
[4]:https://github.com/jedisct1/libsodium
[5]:http://pypy.org/
[6]:http://semver.org/spec/v2.0.0.html

