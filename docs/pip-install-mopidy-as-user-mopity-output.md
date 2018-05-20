# pip-install-mopidy-as-user-mopity-output.md

```shell
The directory '/home/vagrant/.cache/pip/http' or its parent directory is not owned by the current user and the cache has been disabled. Please check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.
The directory '/home/vagrant/.cache/pip' or its parent directory is not owned by the current user and caching wheels has been disabled. check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.
Collecting mopidy
  Downloading https://files.pythonhosted.org/packages/1e/1e/6a64a4a27bda7f025f397e0e11b53168dfc83d613605c1969581bc68f644/Mopidy-2.1.0-py2.py3-none-any.whl (210kB)
    100% |████████████████████████████████| 215kB 1.1MB/s 
Requirement already up-to-date: Pykka>=1.1 in /usr/lib/python2.7/dist-packages (from mopidy)
Collecting setuptools (from mopidy)
  Downloading https://files.pythonhosted.org/packages/7f/e1/820d941153923aac1d49d7fc37e17b6e73bfbd2904959fffbad77900cf92/setuptools-39.2.0-py2.py3-none-any.whl (567kB)
    100% |████████████████████████████████| 573kB 489kB/s 
Collecting requests>=2.0 (from mopidy)
  Downloading https://files.pythonhosted.org/packages/49/df/50aa1999ab9bde74656c2919d9c0c085fd2b3775fd3eca826012bef76d8c/requests-2.18.4-py2.py3-none-any.whl (88kB)
    100% |████████████████████████████████| 92kB 1.1MB/s 
Collecting tornado>=3.2 (from mopidy)
  Downloading https://files.pythonhosted.org/packages/cf/d1/3be271ae5eba9fb59df63c9891fdc7d8044b999e8ac145994cdbfd2ae66a/tornado-5.0.2.tar.gz (506kB)
    100% |████████████████████████████████| 512kB 450kB/s 
Collecting certifi>=2017.4.17 (from requests>=2.0->mopidy)
  Downloading https://files.pythonhosted.org/packages/7c/e6/92ad559b7192d846975fc916b65f667c7b8c3a32bea7372340bfe9a15fa5/certifi-2018.4.16-py2.py3-none-any.whl (150kB)
    100% |████████████████████████████████| 153kB 1.3MB/s 
Collecting chardet<3.1.0,>=3.0.2 (from requests>=2.0->mopidy)
  Downloading https://files.pythonhosted.org/packages/bc/a9/01ffebfb562e4274b6487b4bb1ddec7ca55ec7510b22e4c51f14098443b8/chardet-3.0.4-py2.py3-none-any.whl (133kB)
    100% |████████████████████████████████| 143kB 1.1MB/s 
Collecting idna<2.7,>=2.5 (from requests>=2.0->mopidy)
  Downloading https://files.pythonhosted.org/packages/27/cc/6dd9a3869f15c2edfab863b992838277279ce92663d334df9ecf5106f5c6/idna-2.6-py2.py3-none-any.whl (56kB)
    100% |████████████████████████████████| 61kB 3.6MB/s 
Collecting urllib3<1.23,>=1.21.1 (from requests>=2.0->mopidy)
  Downloading https://files.pythonhosted.org/packages/63/cb/6965947c13a94236f6d4b8223e21beb4d576dc72e8130bd7880f600839b8/urllib3-1.22-py2.py3-none-any.whl (132kB)
    100% |████████████████████████████████| 133kB 1.1MB/s 
Collecting futures (from tornado>=3.2->mopidy)
  Downloading https://files.pythonhosted.org/packages/2d/99/b2c4e9d5a30f6471e410a146232b4118e697fa3ffc06d6a65efde84debd0/futures-3.2.0-py2-none-any.whl
Collecting singledispatch (from tornado>=3.2->mopidy)
  Downloading https://files.pythonhosted.org/packages/c5/10/369f50bcd4621b263927b0a1519987a04383d4a98fb10438042ad410cf88/singledispatch-3.4.0.3-py2.py3-none-any.whl
Collecting backports_abc>=0.4 (from tornado>=3.2->mopidy)
  Downloading https://files.pythonhosted.org/packages/7d/56/6f3ac1b816d0cd8994e83d0c4e55bc64567532f7dc543378bd87f81cebc7/backports_abc-0.5-py2.py3-none-any.whl
Collecting six (from singledispatch->tornado>=3.2->mopidy)
  Downloading https://files.pythonhosted.org/packages/67/4b/141a581104b1f6397bfa78ac9d43d8ad29a7ca43ea90a2d863fe3056e86a/six-1.11.0-py2.py3-none-any.whl
Installing collected packages: setuptools, certifi, chardet, idna, urllib3, requests, futures, six, singledispatch, backports-abc, tornado, mopidy
  Found existing installation: setuptools 20.7.0
    Not uninstalling setuptools at /usr/lib/python2.7/dist-packages, outside environment /usr
  Found existing installation: chardet 2.3.0
    Not uninstalling chardet at /usr/lib/python2.7/dist-packages, outside environment /usr
  Found existing installation: idna 2.0
    Not uninstalling idna at /usr/lib/python2.7/dist-packages, outside environment /usr
  Found existing installation: urllib3 1.13.1
    Not uninstalling urllib3 at /usr/lib/python2.7/dist-packages, outside environment /usr
  Found existing installation: requests 2.9.1
    Not uninstalling requests at /usr/lib/python2.7/dist-packages, outside environment /usr
  Found existing installation: six 1.10.0
    Not uninstalling six at /usr/lib/python2.7/dist-packages, outside environment /usr
  Found existing installation: tornado 4.2.1
    Not uninstalling tornado at /usr/lib/python2.7/dist-packages, outside environment /usr
  Running setup.py install for tornado ... done
Successfully installed backports-abc-0.5 certifi-2018.4.16 chardet-3.0.4 futures-3.2.0 idna-2.6 mopidy-2.1.0 requests-2.18.4 setuptools-39.2.0 singledispatch-3.4.0.3 six-1.11.0 tornado-5.0.2 urllib3-1.22
You are using pip version 8.1.1, however version 10.0.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
```
