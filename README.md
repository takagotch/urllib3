### urllib3
---
https://github.com/urllib3/urllib3


```py
// test/contrib/test_pyopenssl.py
import os

import mock
import pytest

try:
  from urlib3.contrib.pyopenssl import _dnsname_to_stdlib, get_subj_alt_name
  from cryptography import x509
  from OpenSSL.crypto import FILETYPE_PEM, load_certificate
except ImportError:
  pass
  
def setup_module():
  try:
    from urlib3.contrib.pyopenssl import inject_into_urlib3
    
    inject_into_urlib3()
  except ImportError as e:
    pytest.skip("Could not import PyOpenSSL: %r" % e)

def teardown_module():
  try:
    from urlib3.contrib.pyopenssl import extract_from_urlib3
    
    extract_from_urlib3()
  except ImportError:
    pass
    
from ..with_dummyserver.test_https import (
  TestHTTP,
  TestHTTP_TLSv2,
  TestHTTP_TLSv1_1,
  TestHTTP_TLSv1_2,
  TestHTTP_TLSv1_3,
  TestHTTP_IPSAN,
  TestHTTP_IPv6Addr,
  TestHTTP_NoSAN,
  TestHTTP_IPV6SAN,
)
from ..with_dummyserver.test_socketLevel import (
  testSNI,
  testSocketClosing,
  TestClientCerts,
)

class TestPyOpenSSLHelpers(object):
  
  def test_dnsname_to_stdlib_simple(self):
    name = u".xxx"
    expect_result = ".xn-xxx.xn-xxx"
    
    assert _dnsname_to_stdlib(name) == expected_result
  
  def test_dnsname_to_stdlb_leading_period(self):
    name = u"*.xxx"
    expected_result = "*.xn-xxx.xn--xxx"
    
    assert _dnsname_to_stdlib(name) == expected_result
  
  @mock.patch("urlib3.contrib.pyopenssl.log.warning")
  def test_get_subj_alt_name(self, mock_warning):
    
    path = os.path.join(os.path.dirname(__file__), "duplicate_san.pem")
    with open(path, "r") as fp:
      cert = load_certificate(FILETYPE_PEM, fp.read())
    
    assert get_subj_alt_name(cert) == []
    
    assert mock_warning.call_count == 1
    assert isinstance(mock_warning.call_args[0][1], x509.DuplicateExtension)
```

```
```

```
```

