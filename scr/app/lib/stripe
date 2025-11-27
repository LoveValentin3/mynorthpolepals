import Stripe from 'stripe';

export const stripe = new Stripe(process.env.STRIPE_SECRET_KEY!, {
  apiVersion: '2023-10-16',
  typescript: true,
});

export const PRICING = {
  MONTHLY: {
    priceId: process.env.STRIPE_MONTHLY_PRICE_ID!,
    amount: 1499, // $14.99
    interval: 'month',
  },
  YEARLY: {
    priceId: process.env.STRIPE_YEARLY_PRICE_ID!,
    amount: 9999, // $99.99
    interval: 'year',
  },
  LIFETIME: {
    priceId: process.env.STRIPE_LIFETIME_PRICE_ID!,
    amount: 29900, // $299
    interval: 'one_time',
  },
};

export const ADDONS = {
  NICE_CERTIFICATE: { amount: 999, name: 'Nice List Certificate' },
  FRIENDSHIP_CERTIFICATE: { amount: 799, name: 'Friendship Certificate' },
  VIDEO_BUNDLE: { amount: 1999, name: 'Extra Video Bundle (5 videos)' },
  PHYSICAL_LETTER: { amount: 2499, name: 'Physical Letter Package' },
};

export async function createCheckoutSession(
  customerId: string | null,
  priceId: string,
  metadata: Record<string, string>
): Promise<Stripe.Checkout.Session> {
  const session = await stripe.checkout.sessions.create({
    customer: customerId || undefined,
    payment_method_types: ['card'],
    line_items: [
      {
        price: priceId,
        quantity: 1,
      },
    ],
    mode: priceId === PRICING.LIFETIME.priceId ? 'payment' : 'subscription',
    success_url: `${process.env.NEXT_PUBLIC_APP_URL}/parent/dashboard?success=true`,
    cancel_url: `${process.env.NEXT_PUBLIC_APP_URL}/parent/subscription?canceled=true`,
    metadata,
  });

  return session;
}

export async function createCustomer(email: string, name: string): Promise<string> {
  const customer = await stripe.customers.create({
    email,
    name,
  });

  return customer.id;
}
