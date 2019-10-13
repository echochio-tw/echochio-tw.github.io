---
layout: post
title: opencart 加顯示發票功能
date: 2018-07-25
tags: opencart
---

Opencart 加功能 - 發票功能
===================================================
台灣發票是 -->  2 個英文 + 8 個數字

那發票的 invoice_prefix 可以利用成兩個英文首碼

但是我懶沒用 .... 哈哈 ... 理論是要用的 ....XD

# 首先去 資料庫將invoice_no 由 int 改為 string ... 這就不寫了

# 人類為啥會進步 ... 因為  ... 懶

# 廢話不說了 ...開始 ...

左選單加個發票功能 (多了 sale/invoice )

admin/controller/common/column_left.php
```

                        // Sales
                        $sale = array();

                        if ($this->user->hasPermission('access', 'sale/order')) {
                                $sale[] = array(
                                        'name'     => $this->language->get('text_order'),
                                        'href'     => $this->url->link('sale/order', 'user_token=' . $this->session->data['user_token']),
                                        'children' => array()
                                );
                        }

                        if ($this->user->hasPermission('access', 'sale/invoice')) {
                                $sale[] = array(
                                        'name'     => $this->language->get('text_invoice'),
                                        'href'     => $this->url->link('sale/invoice', 'user_token=' . $this->session->data['user_token']),
                                        'children' => array()
                                );
                        }


                        if ($this->user->hasPermission('access', 'sale/return')) {
                                $sale[] = array(
                                        'name'     => $this->language->get('text_return'),
                                        'href'     => $this->url->link('sale/return', 'user_token=' . $this->session->data['user_token']),
                                        'children' => array()
                                );
                        }

```
語言也要有 text_invoice 這參數

admin/language/zh-hk/common/column_left.php

```
$_['text_invoice']                  = '訂單發票';
```

```
<?php
class ControllerSaleInvoice extends Controller {
        private $error = array();

        public function index() {
                $this->load->language('sale/invoice');

                $this->document->setTitle($this->language->get('heading_title'));

                $this->load->model('sale/invoice');

                $this->getList();
        }


        public function edit() {
                $this->load->language('sale/invoice');

                $this->document->setTitle($this->language->get('heading_title'));

                $this->load->model('sale/invoice');

                $this->getForm();
        }

.....

```

nguage('sale/invoice') ..

admin/language/zh-hk/sale/invoice.php

(複製 admin/language/zh-hk/sale/order.php 去修改 ... )

```
$_['heading_title']                    = '訂單發票';
....

```

裡面用到 model('sale/invoice') ...

(複製 admin/model/sale/order.php 去修改 ... 取 invoice_no 為主)

裡面用到 model('sale/invoice') ...

order_info_invoice.twig 裡面讓使用者可輸入 新的發票號碼 

admin/view/template/sale/order_info_invoice.twig

```
                <td>{{ text_invoice }}</td>
                <td id="invoice" class="text-right"><input type="text"  id="get-invoice" name="invoice" value="{{ invoice_no }}" /></td>
                <td style="width: 1%;" class="text-center">
                    <button id="button-invoice" data-loading-text="{{ text_loading }}" data-toggle="tooltip" title="{{ button_save }}" class="btn btn-success btn-xs"><i class="fa fa-cog"></i></button>
              </tr>
              <tr>
```


ller (sale/invoice/createinvoiceno)

我debug 看輸入的 發票號碼 有沒有抓到 ... 加了 alert ... 最後沒拿掉 ...還好吧...

admin/view/template/sale/order_info_invoice.twig
```
$(document).delegate('#button-invoice', 'click', function() {
        alert("發票號碼 : "+document.getElementById('get-invoice').value);
        $.ajax({
                url: 'index.php?route=sale/invoice/createinvoiceno&user_token={{ user_token }}&order_id={{ order_id }}&InvoiceNo='+document.getElementById('get-invoice').value,
                dataType: 'json',
                beforeSend: function() {
                        $('#button-invoice').button('loading');
                },
                complete: function() {
                        $('#button-invoice').button('reset');
                },
                success: function(json) {
                        $('.alert-dismissible').remove();

                        if (json['error']) {
                                $('#content > .container-fluid').prepend('<div class="alert alert-danger alert-dismissible"><i class="fa fa-exclamation-circle"></i> ' + json['error'] + '</div>');
                        }

                        if (json['invoice_no']) {
                                $('#invoice').html(json['invoice_no']);

                                $('#button-invoice').replaceWith('<button disabled="disabled" class="btn btn-success btn-xs"><i class="fa fa-cog"></i></button>');
                        }
                },
                error: function(xhr, ajaxOptions, thrownError) {
                        alert(thrownError + "\r\n" + xhr.statusText + "\r\n" + xhr.responseText);
                }
        });
});
```

那發票丟到 controller (sale/invoice/createinvoiceno)

就是 invoice.php 內的 function createInvoiceNo

然後丟到 model (我用 sale_order 的 createInvoiceNo) 

傳入 order_id , invoice_no 放入資料庫

回傳 model 回傳的 invoice_no 到 view (用 ajax 的 json 回傳)

admin/controller/sale/invoice.php

```
          public function createInvoiceNo() {
                  $json = array();

                  if (isset($this->request->get['order_id'])) {
                          $order_id = $this->request->get['order_id'];
                          // $invoice_no = $this->request->get['InvoiceNo'];
                  } else {
                          $order_id = 0;
                  }


                  if (isset($this->request->get['InvoiceNo'])) {
                          $invoice_no = $this->request->get['InvoiceNo'];
                  } else {
                          $invoice_no = 'NO !!';
                  }


                  if (!$this->user->hasPermission('modify', 'sale/invoice')) {
                          $json['error'] = $this->language->get('error_permission');
                  } else {
                          $this->load->model('sale/order');

                          $invoice_no = $this->model_sale_order->createInvoiceNo($order_id , $invoice_no);

                          if ($invoice_no) {
                                  $json['invoice_no'] = $invoice_no;
                          } else {
                                  $json['error'] = $this->language->get('error_action');
                          }
                  }

                  $this->response->addHeader('Content-Type: application/json');
                  $this->response->setOutput(json_encode($json));
          }
```

發票丟到 model (sale/order/createInvoiceNo)

就是 order.php 內的 function createInvoiceNo

傳入 order_id , invoice_no 放入資料庫 

傳回 放入的 invoice_no 給 controller

```
        public function createInvoiceNo($order_id, $invoice_no) {

                        $this->db->query("UPDATE `" . DB_PREFIX . "order` SET invoice_no = '" . (string)$invoice_no . "', invoice_prefix = '" . $this->db->escape($order_info['invoice_prefix']) . "' WHERE order_id = '" . (int)$order_id . "'");

                        return $order_info['invoice_prefix'] . $invoice_no;
        }
```


<img src="/images/posts/opencart/2.png">

<img src="/images/posts/opencart/3.png">

<img src="/images/posts/opencart/4.png">

<img src="/images/posts/opencart/5.png">

<img src="/images/posts/opencart/6.png">
```

debug 改了些原始碼(我也忘了改了啥 ... 反正可工作) :

admin/controller/sale/invoice.php

admin/view/template/sale/order_info_invoice.twig

admin/model/sale/invoice.php

admin/model/sale/order.php

source 在這邊

```
https://github.com/echochio-tw/echochio-tw.github.io/tree/master/images/opencart-invoice
```
