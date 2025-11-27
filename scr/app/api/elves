import { NextRequest, NextResponse } from 'next/server';
import prisma from '@/lib/prisma';

export async function GET(req: NextRequest) {
  try {
    const { searchParams } = new URL(req.url);
    const gender = searchParams.get('gender');

    const where = gender ? { gender: gender.toUpperCase() } : {};

    const elves = await prisma.elf.findMany({
      where,
      orderBy: { name: 'asc' },
    });

    return NextResponse.json({ elves });
  } catch (error) {
    console.error('Get elves error:', error);
    return NextResponse.json({ error: 'Internal server error' }, { status: 500 });
  }
}
