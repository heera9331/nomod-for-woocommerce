# Nomod For Woocommerce

MARCH 2025

## Your Requirements

I have a quick job of payment gateway integration of nomod.com into my website which built on wordpress.

You have to integrate the nomod payment gateway into my site and if you done nomod payment integration then bid the project and enter "a\*&334" into top of your project so I understand you have completely review the details.

## Proposal for Nomod Payment Gateway Integration in WordPress

a\*&334
I have carefully reviewed your project details, and I am confident that I can successfully integrate the Nomod payment gateway into your WordPress-based eCommerce website. I have previous experience working with WooCommerce and payment gateway integrations, including Stripe, PayPal, and other third-party APIs. I'm comfortable handling API-based payment flows and ensuring a seamless checkout experience for your customers.

## Here is my Approach for this project

### API Integration:

- Generate and secure Nomod API keys.
- Create a custom WooCommerce payment class to handle Nomod API requests.
- Payment Processing:
- Create a dynamic payment link via Nomod.
- Redirect the customer to the Nomod payment link after checkout submission.
- Payment Confirmation:
- Setup webhook to receive payment status updates.
- Update WooCommerce order status based on Nomod's response.
- Testing and Deployment:
- Test in a sandbox environment.
- Ensure secure handling of API keys and payment data.
- Go live after successful testing.

## Sample Code

```php

<?php
/*
 * Plugin Name: Nomod Payment Gateway for WooCommerce
 * Description: Integrate Nomod payment gateway into WooCommerce.
 * Version: 1.0
 * Author: Rohan Sen
 */

if (!defined('ABSPATH')) {
    exit;
}

function init_nomod_payment_gateway() {
    if (!class_exists('WC_Payment_Gateway')) return;

    class WC_Gateway_Nomod extends WC_Payment_Gateway {
        public function __construct() {
            $this->id                 = 'nomod';
            $this->method_title       = 'Nomod Payment';
            $this->method_description = 'Pay securely with Nomod payment gateway';
            $this->has_fields         = false;

            // Load settings
            $this->init_form_fields();
            $this->init_settings();

            $this->title = $this->get_option('title');
            $this->description = $this->get_option('description');
            $this->api_key = $this->get_option('api_key');

            // Save settings
            add_action('woocommerce_update_options_payment_gateways_' . $this->id, array($this, 'process_admin_options'));
        }

        public function init_form_fields() {
            $this->form_fields = array(
                'enabled' => array(
                    'title'   => 'Enable/Disable',
                    'type'    => 'checkbox',
                    'label'   => 'Enable Nomod Payment Gateway',
                    'default' => 'yes'
                ),
                'title' => array(
                    'title'       => 'Title',
                    'type'        => 'text',
                    'description' => 'This controls the title which the user sees during checkout.',
                    'default'     => 'Pay with Nomod',
                    'desc_tip'    => true,
                ),
                'description' => array(
                    'title'       => 'Description',
                    'type'        => 'textarea',
                    'description' => 'This controls the description which the user sees during checkout.',
                    'default'     => 'Pay securely using Nomod.',
                ),
                'api_key' => array(
                    'title'       => 'API Key',
                    'type'        => 'text',
                    'description' => 'Enter your Nomod API key here.',
                ),
            );
        }

        public function process_payment($order_id) {
            $order = wc_get_order($order_id);
            $payment_link = $this->create_nomod_payment($order);

            if ($payment_link) {
                return array(
                    'result'   => 'success',
                    'redirect' => $payment_link,
                );
            } else {
                wc_add_notice('Payment error: Unable to create payment link', 'error');
                return array(
                    'result' => 'failure'
                );
            }
        }

        private function create_nomod_payment($order) {
            $api_url = 'https://api.nomod.com/v1/links';

            $body = json_encode(array(
                'amount' => (int) ($order->get_total() * 100), // Amount in cents
                'currency' => $order->get_currency(),
                'description' => 'Order #' . $order->get_id(),
                'redirect_url' => $this->get_return_url($order),
            ));

            $response = wp_remote_post($api_url, array(
                'method'    => 'POST',
                'body'      => $body,
                'headers'   => array(
                    'Content-Type'  => 'application/json',
                    'Authorization' => 'Bearer ' . $this->api_key,
                ),
                'timeout'   => 45,
            ));

            if (is_wp_error($response)) {
                return false;
            }

            $response_body = json_decode(wp_remote_retrieve_body($response));

            if (isset($response_body->url)) {
                return $response_body->url;
            }

            return false;
        }
    }

    function add_nomod_payment_gateway($methods) {
        $methods[] = 'WC_Gateway_Nomod';
        return $methods;
    }

    add_filter('woocommerce_payment_gateways', 'add_nomod_payment_gateway');
}

add_action('plugins_loaded', 'init_nomod_payment_gateway');
```

## Webhook Setup for Payment Verification:

```php
add_action('woocommerce_api_nomod', 'handle_nomod_webhook');

function handle_nomod_webhook() {
$body = file_get_contents('php://input');
    $event = json_decode($body);

    if ($event && $event->type === 'payment.completed') {
        $order_id = $event->data->metadata->order_id;
        $order = wc_get_order($order_id);

        if ($order) {
            $order->payment_complete();
            $order->add_order_note('Payment successfully processed via Nomod.');
            http_response_code(200);
            exit;
        }
    }

    http_response_code(400);
    exit;

}
```

## Why Choose Me?

Proven experience with WooCommerce payment gateways (Stripe, PayPal, Razorpay).
Strong knowledge of REST APIs and secure payment flows.
Quick turnaround time with a focus on performance and security.
Commitment to testing and debugging to ensure a seamless checkout experience.

## Next Steps

If you’re ready to proceed, I’d be happy to discuss your project requirements in more detail and answer any questions you may have.
Looking forward to working with you!

## Note

I’m new on Upwork, but I have extensive experience in API and payment gateway integration. I’m confident in my ability to deliver high-quality results tailored to your project’s requirements.

I’m based in India and available to work in your time zone (since India and Pakistan have nearly identical time zones). I’m fluent in English and Hindi, so communication will be smooth, and I’ll be able to clarify any details promptly if needed.

I’m excited about the opportunity to work with you — looking forward to discussing your project in more detail!

_Best regards_,

Heera Singh

**Expert WordPress Developer**
