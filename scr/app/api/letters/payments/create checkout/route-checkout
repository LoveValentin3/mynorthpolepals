import { NextRequest, NextResponse } from 'next/server';
import { getServerSession } from 'next-auth';
import { authOptions } from '@/lib/auth';
import { createCheckoutSession, PRICING } from '@/lib/stripe';
import prisma from '@/lib/prisma';
import { z } from 'zod';

const checkoutSchema = z.object({
  tier: z.enum(['MONTHLY', 'YEARLY', 'LIFETIME']),
  addOns: z.array(z.string()).optional(),
});

export async function POST(req: NextRequest) {
  try {
    const session = await getServerSession(authOptions);
    if (!session || session.user.role !== 'PARENT') {
      return NextResponse.json({ error: 'Unauthorized' }, { status: 401 });
    }

    const body = await req.json();
    const validatedData = checkoutSchema.parse(body);

    // Get parent profile
    const parentProfile = await prisma.parentProfile.findFirst({
      where: { userId: session.user.id },
    });

    if (!parentProfile) {
      return NextResponse.json({ error: 'Parent profile not found' }, { status: 404 });
    }

    // Get price ID based on tier
    const priceId = PRICING[validatedData.tier].priceId;

    // Create Stripe checkout session
    const checkoutSession = await createCheckoutSession(
      parentProfile.subscriptionId,
      priceId,
      {
        userId: session.user.id,
        parentProfileId: parentProfile.id,
        tier: validatedData.tier,
        addOns: JSON.stringify(validatedData.addOns || []),
      }
    );

    return NextResponse.json({ 
      sessionId: checkoutSession.id,
      url: checkoutSession.url,
    });
  } catch (error) {
    if (error instanceof z.ZodError) {
      return NextResponse.json(
        { error: 'Invalid input', details: error.errors },
        { status: 400 }
      );
    }

    console.error('Checkout error:', error);
    return NextResponse.json({ error: 'Internal server error' }, { status: 500 });
  }
}
