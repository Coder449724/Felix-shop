const { Client, GatewayIntentBits, Partials, ActionRowBuilder, ButtonBuilder, ButtonStyle, Events } = require('discord.js');
const client = new Client({
  intents: [GatewayIntentBits.Guilds, GatewayIntentBits.GuildMessages, GatewayIntentBits.MessageContent, GatewayIntentBits.DirectMessages],
  partials: [Partials.Channel]
});

const TOKEN = process.env.BOT_TOKEN;

client.once('ready', () => {
  console.log(`Bot ist online als ${client.user.tag}`);
});

client.on('messageCreate', async message => {
  if (message.content === '!shop') {
    const row = new ActionRowBuilder()
      .addComponents(
        new ButtonBuilder()
          .setCustomId('buy_sword')
          .setLabel('Schwert kaufen')
          .setStyle(ButtonStyle.Primary),
        new ButtonBuilder()
          .setCustomId('buy_shield')
          .setLabel('Schild kaufen')
          .setStyle(ButtonStyle.Secondary)
      );

    await message.channel.send({ content: 'Willkommen im Shop! Wähle ein Item:', components: [row] });
  }
});

client.on(Events.InteractionCreate, async interaction => {
  if (!interaction.isButton()) return;

  const user = interaction.user;

  if (interaction.customId === 'buy_sword') {
    await interaction.reply({ content: 'Schwert wurde ausgewählt! Check deine DMs.', ephemeral: true });
    user.send('Danke für deinen Kauf: Schwert! ⚔️');
  } else if (interaction.customId === 'buy_shield') {
    await interaction.reply({ content: 'Schild wurde ausgewählt! Check deine DMs.', ephemeral: true });
    user.send('Danke für deinen Kauf: Schild! 🛡️');
  }
});

client.login(TOKEN);
