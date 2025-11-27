import { NextRequest, NextResponse } from 'next/server';
import { getServerSession } from 'next-auth';
import { authOptions } from '@/lib/auth';
import prisma from '@/lib/prisma';
import { generateElfResponse, moderateContent } from '@/lib/openai';
import { z } from 'zod';

const letterSchema = z.object({
  kidId: z.string(),
  elfId: z.string(),
  content: z.string().min(1).max(5000),
});

// GET all letters for a kid
export async function GET(req: NextRequest) {
  try {
    const session = await getServerSession(authOptions);
    if (!session) {
      return NextResponse.json({ error: 'Unauthorized' }, { status: 401 });
    }

    const { searchParams } = new URL(req.url);
    const kidId = searchParams.get('kidId');

    if (!kidId) {
      return NextResponse.json({ error: 'Kid ID required' }, { status: 400 });
    }

    const letters = await prisma.letter.findMany({
      where: { kidId },
      include: {
        elf: true,
      },
      orderBy: { createdAt: 'desc' },
    });

    return NextResponse.json({ letters });
  } catch (error) {
    console.error('Get letters error:', error);
    return NextResponse.json({ error: 'Internal server error' }, { status: 500 });
  }
}

// POST new letter from kid
export async function POST(req: NextRequest) {
  try {
    const session = await getServerSession(authOptions);
    if (!session) {
      return NextResponse.json({ error: 'Unauthorized' }, { status: 401 });
    }

    const body = await req.json();
    const validatedData = letterSchema.parse(body);

    // Moderate content
    const isSafe = await moderateContent(validatedData.content);
    if (!isSafe) {
      return NextResponse.json(
        { error: 'Content flagged for moderation' },
        { status: 400 }
      );
    }

    // Get kid profile
    const kid = await prisma.kidProfile.findUnique({
      where: { id: validatedData.kidId },
      include: { 
        user: { 
          include: { 
            parentProfile: true 
          } 
        } 
      },
    });

    if (!kid) {
      return NextResponse.json({ error: 'Kid not found' }, { status: 404 });
    }

    // Get elf
    const elf = await prisma.elf.findUnique({
      where: { id: validatedData.elfId },
    });

    if (!elf) {
      return NextResponse.json({ error: 'Elf not found' }, { status: 404 });
    }

    // Create kid's letter
    const kidLetter = await prisma.letter.create({
      data: {
        kidId: validatedData.kidId,
        elfId: validatedData.elfId,
        content: validatedData.content,
        isFromKid: true,
        isAiGenerated: false,
      },
    });

    // Check if should auto-respond with AI
    const shouldAutoRespond = kid.user.parentProfile?.responseMode === 'AI';

    if (shouldAutoRespond) {
      // Get previous conversation for context
      const previousLetters = await prisma.letter.findMany({
        where: {
          kidId: validatedData.kidId,
          elfId: validatedData.elfId,
        },
        orderBy: { createdAt: 'desc' },
        take: 5,
      });

      const context = previousLetters.map(
        (l) => `${l.isFromKid ? 'Kid' : 'Elf'}: ${l.content}`
      );

      // Generate AI response
      const aiResponse = await generateElfResponse(
        kid.name,
        kid.age,
        elf.name,
        elf.personality,
        validatedData.content,
        context
      );

      // Create elf's response
      const elfLetter = await prisma.letter.create({
        data: {
          kidId: validatedData.kidId,
          elfId: validatedData.elfId,
          content: aiResponse,
          isFromKid: false,
          isAiGenerated: true,
        },
      });

      return NextResponse.json({
        kidLetter,
        elfResponse: elfLetter,
      });
    }

    return NextResponse.json({ kidLetter });
  } catch (error) {
    if (error instanceof z.ZodError) {
      return NextResponse.json(
        { error: 'Invalid input', details: error.errors },
        { status: 400 }
      );
    }

    console.error('Create letter error:', error);
    return NextResponse.json({ error: 'Internal server error' }, { status: 500 });
  }
}
