import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

const elves = [
  // Boy Elves
  {
    name: 'Jingles',
    gender: 'BOY',
    emoji: '🧝‍♂️',
    role: 'Toy Workshop Manager',
    personality: 'Cheerful and loves building wooden toys. Always has paint on his hands!',
    likes: 'Woodworking, hot cocoa, jingle bells',
    isPremium: false,
  },
  {
    name: 'Sparkle',
    gender: 'BOY',
    emoji: '✨',
    role: 'Gift Wrapper Expert',
    personality: 'Detail-oriented and creative. Makes the most beautiful bows!',
    likes: 'Ribbons, glitter, making things pretty',
    isPremium: false,
  },
  {
    name: 'Buddy',
    gender: 'BOY',
    emoji: '🎅',
    role: 'Cookie Taste Tester',
    personality: 'Friendly and always hungry. Loves trying new cookie recipes!',
    likes: "Cookies, milk, Mrs. Claus's kitchen",
    isPremium: false,
  },
  {
    name: 'Tinsel',
    gender: 'BOY',
    emoji: '🎄',
    role: 'Tree Decorator',
    personality: 'Artistic and loves all things shiny. Expert at untangling lights!',
    likes: 'Ornaments, lights, decorating',
    isPremium: false,
  },
  {
    name: 'Dash',
    gender: 'BOY',
    emoji: '🦌',
    role: 'Reindeer Caretaker',
    personality: 'Energetic and loves animals. Best friends with all the reindeer!',
    likes: 'Running, carrots, playing with Rudolph',
    isPremium: false,
  },
  {
    name: 'Frosty',
    gender: 'BOY',
    emoji: '⛄',
    role: 'Snow Sculptor',
    personality: 'Cool and creative. Makes amazing ice sculptures!',
    likes: 'Snowball fights, ice skating, winter',
    isPremium: false,
  },
  {
    name: 'Merry',
    gender: 'BOY',
    emoji: '🎵',
    role: 'Carol Singer',
    personality: 'Musical and jolly. Always humming Christmas tunes!',
    likes: 'Singing, bells, spreading cheer',
    isPremium: false,
  },
  {
    name: 'Jolly',
    gender: 'BOY',
    emoji: '😊',
    role: 'Happiness Officer',
    personality: 'Super positive and loves making others smile!',
    likes: 'Jokes, laughter, happy faces',
    isPremium: false,
  },
  {
    name: 'Zippy',
    gender: 'BOY',
    emoji: '⚡',
    role: 'Mail Sorter',
    personality: 'Fast and organized. Reads letters to elves super quickly!',
    likes: 'Speed, efficiency, letters from kids',
    isPremium: false,
  },
  {
    name: 'Chester',
    gender: 'BOY',
    emoji: '📚',
    role: 'Story Keeper',
    personality: 'Wise and loves reading. Knows all the Christmas stories!',
    likes: 'Books, stories, quiet time',
    isPremium: true,
  },

  // Girl Elves
  {
    name: 'Snowflake',
    gender: 'GIRL',
    emoji: '❄️',
    role: 'Winter Magic Specialist',
    personality: 'Gentle and magical. Can make snowflakes dance in the air!',
    likes: 'Winter, magic, dancing snowflakes',
    isPremium: false,
  },
  {
    name: 'Twinkle',
    gender: 'GIRL',
    emoji: '⭐',
    role: 'Star Lighter',
    personality: 'Bright and sparkly. Lights up the North Pole every night!',
    likes: 'Stars, lights, night sky',
    isPremium: false,
  },
  {
    name: 'Cookie',
    gender: 'GIRL',
    emoji: '🍪',
    role: "Baker's Assistant",
    personality: 'Sweet and loves baking. Makes the best gingerbread!',
    likes: 'Baking, decorating cookies, frosting',
    isPremium: false,
  },
  {
    name: 'Holly',
    gender: 'GIRL',
    emoji: '🌿',
    role: 'Wreath Maker',
    personality: 'Nature-loving and crafty. Creates beautiful wreaths!',
    likes: 'Plants, crafts, decorations',
    isPremium: false,
  },
  {
    name: 'Joy',
    gender: 'GIRL',
    emoji: '💝',
    role: 'Gift Selector',
    personality: 'Thoughtful and kind. Knows exactly what everyone wants!',
    likes: 'Helping, gifts, making wishes come true',
    isPremium: false,
  },
  {
    name: 'Belle',
    gender: 'GIRL',
    emoji: '🔔',
    role: 'Bell Ringer',
    personality: 'Musical and elegant. Plays the most beautiful bell melodies!',
    likes: 'Music, bells, performances',
    isPremium: false,
  },
  {
    name: 'Candy',
    gender: 'GIRL',
    emoji: '🍬',
    role: 'Candy Cane Maker',
    personality: 'Sweet and energetic. Loves all things peppermint!',
    likes: 'Candy, sweets, peppermint',
    isPremium: false,
  },
  {
    name: 'Star',
    gender: 'GIRL',
    emoji: '🌟',
    role: 'Wish Granter',
    personality: 'Magical and hopeful. Helps make Christmas wishes come true!',
    likes: 'Dreams, wishes, magic',
    isPremium: false,
  },
  {
    name: 'Ribbon',
    gender: 'GIRL',
    emoji: '🎀',
    role: 'Fashion Designer',
    personality: "Stylish and creative. Designs all the elves' outfits!",
    likes: 'Fashion, sewing, pretty things',
    isPremium: false,
  },
  {
    name: 'Crystal',
    gender: 'GIRL',
    emoji: '💎',
    role: 'Ice Palace Keeper',
    personality: 'Elegant and organized. Keeps the North Pole sparkling!',
    likes: 'Cleanliness, ice sculptures, beauty',
    isPremium: true,
  },
];

async function main() {
  console.log('🎄 Starting database seed...');

  // Clear existing data
  await prisma.letter.deleteMany();
  await prisma.video.deleteMany();
  await prisma.certificate.deleteMany();
  await prisma.gameProgress.deleteMany();
  await prisma.kidProfile.deleteMany();
  await prisma.elf.deleteMany();

  console.log('✅ Cleared existing data');

  // Seed elves
  for (const elf of elves) {
    await prisma.elf.create({
      data: elf,
    });
  }

  console.log(`✅ Created ${elves.length} elves`);

  console.log('🎉 Seed completed successfully!');
}

main()
  .catch((e) => {
    console.error('❌ Seed error:', e);
    process.exit(1);
  })
  .finally(async () => {
    await prisma.$disconnect();
  });
