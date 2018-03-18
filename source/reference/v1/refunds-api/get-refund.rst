.. _v1/refunds-get:

Refunds API v1: Get refund
==========================
``GET`` ``https://api.mollie.com/v1/payments/*paymentId*/refunds/*id*``

Authentication: :ref:`API keys <guides/authentication>`. :ref:`OAuth access tokens <oauth/overview>`

Retrieve a single refund by its ID. Note the original payment's ID is needed as well.

If you do not know the original payment's ID, you can use the :ref:`refunds list endpoint <v1/refunds-list>`.

Parameters
----------
Replace ``paymentId`` in the endpoint URL by the payment's ID, and replace ``id`` by the refund's ID. For example:
``/v1/payments/tr_7UhSN1zuXS/refunds/re_4qqhO89gsT``.

Response
--------
``200`` ``application/json; charset=utf-8``

.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``id``
       | string
     - The refund's unique identifier, for example ``re_4qqhO89gsT``.

   * - | ``payment``
       | object
     - The original payment, as described in :ref:`Get payment <v1/payments-get>`. In the payment object, note the
       following refund related fields.

       .. list-table::
          :header-rows: 0
          :widths: auto

          * - | ``amountRefunded``
              | decimal
            - The total amount in EUR that is already refunded. For some payment methods, this amount may be higher than
              the payment amount, for example to allow reimbursement of the costs for a return shipment to the consumer.

          * - | ``amountRemaining``
              | decimal
            - The remaining amount in EUR that can be refunded.

   * - | ``amount``
       | decimal
     - The amount refunded to the consumer with this refund.

   * - | ``description``
       | string
     - The description of the refund that may be shown to the consumer, depending on the payment method used.

   * - | ``status``
       | string
     - Since refunds may be delayed for certain payment methods, the refund carries a status field.

       Possible values:

       * ``queued`` The refund will be processed once you have enough balance. You can still cancel this refund.
       * ``pending`` The refund will be processed soon (usually the next business day). You can still cancel this
         refund.
       * ``processing`` The refund is being processed. Cancellation is no longer possible.
       * ``refunded`` The refund has been paid out to the consumer.
       * ``failed`` The refund has failed during processing.

   * - | ``refundedDatetime``
       | datetime
     - The date and time the refund was issued, in `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format.

Example
-------

Request
^^^^^^^
.. code-block:: bash

   curl -X GET https://api.mollie.com/v1/payments/tr_WDqYK6vllg/refunds/re_4qqhO89gsT \
       -H "Authorization: Bearer test_dHar4XY7LxsDOtmnkVtjNVWXLSlXsM"

Response
^^^^^^^^
.. code-block:: http

   HTTP/1.1 200 OK
   Content-Type: application/json; charset=utf-8

   {
       "id": "re_4qqhO89gsT",
       "payment": {
           "resource": "payment",
           "id": "tr_WDqYK6vllg",
           "mode": "test",
           "createdDatetime": "2018-03-14T07:58:33.0Z",
           "status": "refunded",
           "amount": "35.07",
           "amountRefunded": "5.95",
           "amountRemaining": "54.12",
           "description": "Order",
           "method": "ideal",
           "metadata": {
               "order_id": "33"
           },
           "details": {
               "consumerName": "Hr E G H K\u00fcppers en\/of MW M.J. K\u00fcppers-Veeneman",
               "consumerAccount": "NL53INGB0654422370",
               "consumerBic": "INGBNL2A"
           },
           "locale": "nl",
           "links": {
               "webhookUrl": "https://webshop.example.org/payments/webhook",
               "redirectUrl": "https://webshop.example.org/order/33/",
               "refunds": "https://api.mollie.com/v1/payments/tr_WDqYK6vllg/refunds"
           }
       },
       "amount": "5.95",
       "status": "pending",
       "refundedDatetime": "2018-03-14T17:00:50.0Z",
       "description": "Refund of order",
       "links": {
           "self": "https://api.mollie.dev/v1/payments/tr_WDqYK6vllg/refunds/re_4qqhO89gsT"
       }
   }