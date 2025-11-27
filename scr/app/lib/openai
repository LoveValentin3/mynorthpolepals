import OpenAI from 'openai';

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

export async function generateElfResponse(
  kidName: string,
  kidAge: number,
  elfName: string,
  elfPersonality: string,
  letterContent: string,
  previousContext?: string[]
): Promise<string> {
  const systemPrompt = `You are ${elfName}, a magical elf from the North Pole. 
Your personality: ${elfPersonality}
You are writing to ${kidName}, who is ${kidAge} years old.

IMPORTANT RULES:
- Keep responses warm, magical, and age-appropriate
- Use simple language for younger kids
- Include emojis and Christmas themes (🎄 ⛄ 🎅 ✨)
- Keep responses between 100-200 words
- Always be encouraging and positive
- Mention North Pole activities and elf life
- Ask engaging questions to keep conversation going
- Never mention anything scary or inappropriate
- Keep the Christmas magic alive!`;

  const userPrompt = `${kidName} wrote to you:
"${letterContent}"

${previousContext ? `Previous conversation context:\n${previousContext.join('\n')}` : ''}

Write a warm, magical response from ${elfName} the elf.`;

  try {
    const completion = await openai.chat.completions.create({
      model: process.env.OPENAI_MODEL || 'gpt-4-turbo-preview',
      messages: [
        { role: 'system', content: systemPrompt },
        { role: 'user', content: userPrompt },
      ],
      temperature: 0.8,
      max_tokens: 300,
    });

    return completion.choices[0]?.message?.content || 'Ho ho ho! I received your letter! 🎄';
  } catch (error) {
    console.error('OpenAI API Error:', error);
    throw new Error('Failed to generate elf response');
  }
}

export async function moderateContent(content: string): Promise<boolean> {
  try {
    const moderation = await openai.moderations.create({
      input: content,
    });

    return !moderation.results[0]?.flagged;
  } catch (error) {
    console.error('Moderation Error:', error);
    return true; // Allow content if moderation fails
  }
}
