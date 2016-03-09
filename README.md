### TracRPC

The purpose of this class is to interact with the Trac API from a remote
location by remote procedure calls.

Trac is a project management and bug/issue tracking system.
http://trac.edgewall.org/

Trac by itself does not provide an API. You must install the XmlRpcPlugin. Trac
then provides anonymous and authenticated access to the API via two protocols
XML-RPC and JSON-RPC.
http://trac-hacks.org/wiki/XmlRpcPlugin/

### Requirements

* PHP 5.3.0 or higher
* The PHP Extensions "JSON" and "cURL" are required.
* Trac with XmlRpcPlugin

### Features

* Make Requests to the TRAC API

### Installation

a) Download the ZIP from Github, then extract the library file and include it.

b) Installation via Composer

To add PHPTracRPC as a local, per-project dependency to your project, simply add `jakoch/php-trac-rpc` to your project's `composer.json` file.

    {
        "require": {
            "jakoch/php-trac-rpc": "dev-master"
        }
    }

### Usage

#### Step 1: include library

When you installed via Composer, please include the Composer Autoloader first and then instantiate the TracRPC class.

```
include __DIR__.'/vendor/autoload.php';
```

When you fetched the zip file, please include the lib directly.

```
include __DIR__.'/lib/TracRPC.php';
```

#### Step 2: setup credentials and instantiate TracRPC

```
$credentials = array('username' => 'username', 'password' => 'password');

$trac = new \TracRPC\TracRPC('http://trac.example.com/login/jsonrpc', $credentials);
```

#### Step 3: do some requests 

##### Single Call Example

```
$result = $trac->getTicket('32');

if ($result === false) {
    die('ERROR: '.$trac->getErrorMessage());
} else {
    var_dump($result);
}
```

##### Multi Call Example

```
$trac->setMultiCall(true);

$ticket = $trac->getTicket('32');
$attachments = $trac->getTicketAttachments('list', '32');

$trac->doRequest();

$ticket = $trac->getResponse($ticket);
$attachments = $trac->getResonse($attachments);
var_dump($ticket, $attachments);
```

### Request Methods

1. getRecentChangedWikiPages($date = 0)
2. getWikiPage($name = '', $version = 0, $raw = true)
3. getWikiPageInfo($name = '', $version = 0)
4. getWikiPages()
5. getRecentChangedTickets($date = 0)
6. getTicket($id = '')
7. getTicketFields()
8. getTicketChangelog($id = '', $when = 0)
9. getTicketActions($id = '')
10. getWikiAttachments($action = 'list', $name = '', $file = '')
11. getTicketAttachments($action = 'list', $id = '', $file = '', $desc = '', $replace = true)
12. getWikiUpdate($action = 'create', $name = '', $page = '', $data = array())
13. getTicketUpdate($action = 'create', $id = '', $data = array())
14. getTicketSearch($query = '')
15. getTicketComponent($action = 'get_all', $name = '', $attr = array())
16. getTicketMilestone($action = 'get_all', $name = '', $attr = array())
17. getTicketPriority($action = 'get_all', $name = '', $attr = '')
18. getTicketResolution($action = 'get_all', $name = '', $attr = '')
19. getTicketSeverity($action = 'get_all', $name = '', $attr = '')
20. getTicketType($action = 'get_all', $name = '', $attr = '')
21. getTicketVersion($action = 'get_all', $name = '', $attr = array())
22. getTicketStatus()
23. getSearch($query = '', $filter = array())
24. getWikiTextToHTML($text = '')
25. getSearchFilters()
26. getApiVersion()

### Example: Ajax Request to Trac's JSON-RPC Interface

```
<html>
<head>
   <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.7/jquery.min.js"></script>
   <script>
      $(document).ready(function(){
         $.ajax({
            url: 'http://trac.domain.com/jsonrpc',
            data: JSON.stringify( {method:'ticket.milestone.get', params:['Milstone 1.2.3']} ),
            type: "POST",
            contentType: "application/json",
            dataType:"json",
          success: function (result) {
                 alert("OK");
          },
          error: function (err,status,thrown) {
                 alert("Error: " + err + "! STATUS: " + status + "! " + thrown );
          },
          complete: function (xhr,status) {
                 alert('Completo!  Status: '+ status);
                 data = $.parseJSON(xhr.responseText);
                 console.info(data);
                 /*alert(data);*/
          }
         });
      });
  </script>
</head>
<body>
  <h1>Ajax Request to Trac's JSON-RPC Interface</h1>
</body>
</html>
```
