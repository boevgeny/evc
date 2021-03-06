#########################################################
evc - Simple client for the EVE Python REST API Framework
#########################################################

Example of usage
################

First, run the server "Eve"
===========================

::

    Python 2.7.5 (default, Mar  9 2014, 22:15:05)
    [GCC 4.2.1 Compatible Apple LLVM 5.0 (clang-500.0.68)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    >>> from eve import Eve
    >>> settings = {'XML': False, 'JSON': True, 'SERVER_NAME': '127.0.0.1:5555', 'MONGO_DBNAME': 'evc_test'}
    >>> settings['RESOURCE_METHODS'] = ['GET', 'POST', 'DELETE']
    >>> settings['ITEM_METHODS'] = ['GET', 'PATCH', 'PUT', 'DELETE']
    >>> settings['DOMAIN'] = {'persons': {'versioning': True, 'allow_unknown': True, 'schema': {}}}
    >>> app = Eve(settings=settings)
    >>> app.run()
     * Running on http://127.0.0.1:5555/ (Press CTRL+C to quit)

Second, run the client "Evc"
============================

::

    Python 2.7.5 (default, Mar  9 2014, 22:15:05)
    Type "copyright", "credits" or "license" for more information.

    IPython 5.1.0 -- An enhanced Interactive Python.
    ?         -> Introduction and overview of IPython's features.
    %quickref -> Quick reference.
    help      -> Python's own help system.
    object?   -> Details about 'object', use 'object??' for extra details.

    In [1]: from evc import Evc

    In [2]: REST = Evc('http://127.0.0.1:5555')

    In [3]: type(REST.response)
    Out[3]: NoneType

    In [4]: REST.get()
    Out[4]: {u'_links': {u'child': [{u'href': u'persons', u'title': u'persons'}]}}

    In [5]: type(REST.response)
    Out[5]: requests.models.Response

    In [6]: REST.response.text
    Out[6]: u'{"_links": {"child": [{"href": "persons", "title": "persons"}]}}'

    In [7]: REST.response.json
    Out[7]: <bound method Response.json of <Response [200]>>

    In [8]: REST.response.json()
    Out[8]: {u'_links': {u'child': [{u'href': u'persons', u'title': u'persons'}]}}

    In [9]: REST.post('persons', {'name': 'Bob', 'email': 'bob@bobmail.com'})
    Out[9]:
    {u'_created': u'Tue, 25 Oct 2016 10:46:39 GMT',
     u'_etag': u'90d02265b8d7c832247fcaeacf11d1f4bbf8f0bc',
     u'_id': u'580f380fa7a3497495d72bd4',
     u'_latest_version': 1,
     u'_links': {u'self': {u'href': u'persons/580f380fa7a3497495d72bd4',
       u'title': u'Person'}},
     u'_status': u'OK',
     u'_updated': u'Tue, 25 Oct 2016 10:46:39 GMT',
     u'_version': 1}

    In [10]: REST.patch('persons', '580f380fa7a3497495d72bd4', '90d02265b8d7c832247fcaeacf11d1f4bbf8f0bc', {'name': 'Bob', 'email': 'bob2@bobmail.com'})
    Out[10]:
    {u'_created': u'Tue, 25 Oct 2016 10:46:39 GMT',
     u'_etag': u'50da6d8e759eeb0a32eaf0bd820dcfb729cdf5d9',
     u'_id': u'580f380fa7a3497495d72bd4',
     u'_latest_version': 2,
     u'_links': {u'self': {u'href': u'persons/580f380fa7a3497495d72bd4',
       u'title': u'Person'}},
     u'_status': u'OK',
     u'_updated': u'Tue, 25 Oct 2016 10:48:20 GMT',
     u'_version': 2}


    In [11]: REST.delete('persons', '580f380fa7a3497495d72bd4', '50da6d8e759eeb0a32eaf0bd820dcfb729cdf5d9')
    Out[11]: u''

    In [12]: REST.get('persons', '580f380fa7a3497495d72bd4')
    Out[12]:
    {u'_items': [],
     u'_links': {u'parent': {u'href': u'/', u'title': u'home'},
      u'self': {u'href': u'persons', u'title': u'persons'}},
     u'_meta': {u'max_results': 25, u'page': 1, u'total': 0}}
