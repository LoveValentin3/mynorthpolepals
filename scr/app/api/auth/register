import { NextRequest, NextResponse } from 'next/server';
import { hash } from 'bcryptjs';
import { nanoid } from 'nanoid';
import prisma from '@/lib/prisma';
import { z } from 'zod';

const registerSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
  name: z.string().min(2),
  phone: z.string().optional(),
  userType: z.enum(['PARENT', 'KID']),
  kidData: z.object({
    username: z.string().min(3),
    age: z.number().min(3).max(12),
  }).optional(),
});

export async function POST(req: NextRequest) {
  try {
    const body = await req.json();
    const validatedData = registerSchema.parse(body);

    // Check if user exists
    const existingUser = await prisma.user.findUnique({
      where: { email: validatedData.email },
    });

    if (existingUser) {
      return NextResponse.json(
        { error: 'User already exists' },
        { status: 400 }
      );
    }

    // Hash password
    const hashedPassword = await hash(validatedData.password, 12);

    // Create user
    const user = await prisma.user.create({
      data: {
        email: validatedData.email,
        password: hashedPassword,
        name: validatedData.name,
        phone: validatedData.phone,
        role: validatedData.userType,
      },
    });

    // Create parent profile if parent
    if (validatedData.userType === 'PARENT') {
      await prisma.parentProfile.create({
        data: {
          userId: user.id,
          referralCode: nanoid(10),
        },
      });
    }

    // Create kid profile if kid
    if (validatedData.userType === 'KID' && validatedData.kidData) {
      await prisma.kidProfile.create({
        data: {
          userId: user.id,
          username: validatedData.kidData.username,
          name: validatedData.name,
          age: validatedData.kidData.age,
        },
      });
    }

    return NextResponse.json(
      {
        message: 'User created successfully',
        user: {
          id: user.id,
          email: user.email,
          name: user.name,
          role: user.role,
        },
      },
      { status: 201 }
    );
  } catch (error) {
    if (error instanceof z.ZodError) {
      return NextResponse.json(
        { error: 'Invalid input', details: error.errors },
        { status: 400 }
      );
    }

    console.error('Registration error:', error);
    return NextResponse.json(
      { error: 'Internal server error' },
      { status: 500 }
    );
  }
}
