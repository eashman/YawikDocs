.. index:: Applications

Applications
------------

.. raw:: html

    <div style="float:right; width: 50%">
    <img src="https://www.transifex.com/projects/p/yawik/resource/applications/chart/image_png"/>
    <br/>translation state of Applications module
    </div>

the application module offers an application formular, a list of applications and
a detail view of an application. Depending on the users privileges the detail
view offers a way to invite or reject an applicant, to rate an application or to
forward an application by email.

If the :ref:`PDF` Module is installed, the Application can be downloaded as a PDF
document with attachments and social profiles embedded.

Options
^^^^^^^

You can configure the possible mime-types or the maximum size of attachments by copying the applicationOptions_ into
your config/autoload directory. Remove the ".dist" extention and adjust the values.

.. _applicationOptions: https://github.com/cross-solution/YAWIK/blob/develop/module/Applications/config/applications.forms.global.php.dist

.. code-block:: php

  $options = array(
        /*
        * maximum size in bytes of an uploaded attachment, default 5MB
        */
        'attachmentsMaxSize' => '5000000',

        /*
        * allowed Mime-Type of an uploaded Attachment, default images, *.PDF, *.DOC, *.ODT
        */
        'attachmentsMimeType' => array('image','application/pdf', 'application/vnd.oasis.opendocument.text', 'application/msword'),


        /*
        * maximum amount of uploaded attachments
        */
        'attachmentsCount' => 5,

        /*
        * maximum size in bytes of an uploaded contact photo. default 500kB
        */
        'contactImageMaxSize' => '500000',

        /*
        * allowed Mime-Type of an uploaded contact photo
        */
        'contactImageMimeType' => array('image'),

        /*
        * allowed Mime-Types, images, plain text, *.PDF, *.DOC, *.ODT
        */
        'allowedMimeTypes' => array('image','text','application/pdf', 'application/vnd.oasis.opendocument.text', 'application/msword'),

    );


AbstractIdentifiableModificationDateAwareEntity

Events
^^^^^^

you can attach Listeners to the following events

+----------------------------------------+---------------------------+-----------------------------------------------------------------------+
|Name                                    |                           | description                                                           |
+========================================+===========================+=======================================================================+
| EVENT_APPLICATION_POST_CREATE          | application.post.create   | Thrown, after an application was saved in the Database                |
+----------------------------------------+---------------------------+-----------------------------------------------------------------------+
| EVENT_APPLICATION_PRE_DELETE           | application.pre.delete    | Thrown, befor an application is removed from the Database             |
+----------------------------------------+---------------------------+-----------------------------------------------------------------------+
| EVENT_APPLICATION_STATUS_CHANGE        | application.status.change | Thrown, befor an application is removed from the Database             |
+----------------------------------------+---------------------------+-----------------------------------------------------------------------+

API
^^^

It is possible to create an application through a POST request to
`api/apply` passing in the apply id of the job ad as query parameter.

The data must be sent with the content type `multipart/form-data`

+-----------------------------------------------+------------------------------------------------------------------+
| Field                                         | Value                                                            |
+===============================================+==================================================================+
| Contact                                       |                                                                  |
+-----------------------------------------------+------------------------------------------------------------------+
| contact[gender]                               |                                                                  |
+-----------------------------------------------+------------------------------------------------------------------+
| contact[first_name]                           | First name                                                       |
+-----------------------------------------------+------------------------------------------------------------------+
| contact[last_name]                            | Last name                                                        |
+-----------------------------------------------+------------------------------------------------------------------+
| contact[birthday]                             | YYYY-mm-dd                                                       |
+-----------------------------------------------+------------------------------------------------------------------+
| contact[street]                               |                                                                  |
+-----------------------------------------------+------------------------------------------------------------------+
| contact[house_number]                         |                                                                  |
+-----------------------------------------------+------------------------------------------------------------------+
| contact[postal_code]                          |                                                                  |
+-----------------------------------------------+------------------------------------------------------------------+
| contact[city]                                 |                                                                  |
+-----------------------------------------------+------------------------------------------------------------------+
| contact[country]                              |                                                                  |
+-----------------------------------------------+------------------------------------------------------------------+
| contact[email]                                |                                                                  |
+-----------------------------------------------+------------------------------------------------------------------+
| contact[image]                                | user image (avatar) (must be an image)                           |
+-----------------------------------------------+------------------------------------------------------------------+
| General application data                      |                                                                  |
+-----------------------------------------------+------------------------------------------------------------------+
| summary                                       | The cover letter                                                 |
+-----------------------------------------------+------------------------------------------------------------------+
| Facts                                         |                                                                  |
+-----------------------------------------------+------------------------------------------------------------------+
| facts[expected_salary]                        |                                                                  |
+-----------------------------------------------+------------------------------------------------------------------+
| facts[earliest_starting_date]                 |                                                                  |
+-----------------------------------------------+------------------------------------------------------------------+
| facts[driving_license]                        | Possible values: 0, 1, yes, no                                   |
+-----------------------------------------------+------------------------------------------------------------------+
| Attachments                                   |                                                                  |
+-----------------------------------------------+------------------------------------------------------------------+
| attachments[]                                 | One or multiple files                                            |
+-----------------------------------------------+------------------------------------------------------------------+



Every property of an application and its embedded documents can be send using the above mapping stategy.
Field name `attachments[]` sends a file as attachment for example.

The response is a json string. The complete application entity is returned. The url to track the progress
of the application process is also returned in a key named `track`.

.. code-block:: sh

    # On success (HTTP-Code: 200)
    {
        "status": "OK",
        "entity": {
            "resource_id": "Entity/Application",
            "job": "5c5abf660fc61f047c063b28",
            "user": "token:****************",
            "status": null,
            "contact": {
                "birth_day": null,
                "birth_month": null,
                "birth_year": null,
                "email": null,
                "is_email_verified": null,
                "gender": null,
                "first_name": "Firstname",
                "house_number": null,
                "last_name": null,
                "display_name": null,
                "phone": null,
                "postal_code": null,
                "city": null,
                "image": "http(s)://yawikserver.tld/file/Applications.Attachment/user-image.png",
                "street": null,
                "country": null
            },
            "summary": null,
            "facts": {},
            "cv": {},
            "attachments": [
                "/file/Applications.Attachment/some-attachment.doc",
                "/file/Applications.Attachment/other-attachment.pdf"
            ],
            "profiles": {},
            "attributes": {},
            "id": null,
            "date_created": null,
            "date_modified": null
        },
        "track": "http(s)://yawikserver.tld/de/applications/ID?token=*********"
    }

    # on Failure
    # Either HTTP-Code 400 (No job for the apply id or invalid application data)
    # or HTTP-Code 405 (Invalid request method)

    { "status": "Error", "message": "Meaningful error message" }

Examples
========

Postman
~~~~~~~

To try out the API it is best to use an application which is capable of sending post
requests with file uploads, such as Postman_.

.. figure:: ../../images/yawik-application-api-postman.png
    :scale: 100%
    :align: right

    Postman screenshot


.. _Postman: https://www.postman.com/

cURL
~~~~

But it is also possible to use a cURL command:

.. code-block:: sh

    curl -X POST \
        'http://php7-mg:8080/api/apply?applyId=5ec7aa95af2a2349123cc59f' \
        -H 'Content-Type: application/x-www-form-urlencoded' \
        -H 'Postman-Token: 7a60e36f-d2c3-4d41-85d0-899f4810bd26' \
        -H 'cache-control: no-cache' \
        -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
        -F 'contact[first_name]=John' \
        -F 'contact[last_name]=Doe'
        -F 'contact[image]=@/path/to/an/image.png' \
        -F 'attachments[]=@/path/to/a/file' \
        -F 'attachments[]=@/path/to/another/file'


Workflow
^^^^^^^^

