const Discord = require('discord.js');
const bot = new Discord.Client


const PREFIX = '!';

var version='1.0.1';




bot.on('ready', ()=>{
 console.log('AlmogRp-bot');
 bot.user.setActivity('משחק ביד', {type: 'STREAMING'})
})
bot.on('message', message => {
    // If the message is "what is my avatar"
    if (message.content === '!avatar') {
      // Send the user's avatar URL
      message.channel.send(message.author.displayAvatarURL());
    }
  });
  
  bot.on('ready', () => {
    console.log('I am ready!');
  });
  

  bot.on('guildMemberAdd', member => {
    const channel = member.guild.channels.cache.find(ch => ch.name === '【👋】ברוכים-הבאים');
    if (!channel) return;
    channel.send(`ברוך הבא לסרבר, ${member}`);
  });
  

bot.login(process.env.BOT_TOKEN);

bot.on('ready', () => console.log(`${bot.user.tag} has logged in.`));

const usersMap = new Map();
const LIMIT = 5;
const TIME = 30000;
const DIFF = 10000;


bot.on('message', message => {
  if(message.author.bot) return;
  if(usersMap.has(message.author.id)) {
    const userData = usersMap.get(message.author.id);
    const { lastMessage, timer } = userData;
    const difference = message.createdTimestamp - lastMessage.createdTimestamp;
    let msgCount = userData.msgCount;
    console.log(difference);
    if(difference > DIFF) {
      clearTimeout(timer);
      console.log('Cleared timeout');
      userData.msgCount = 1;
      userData.lastMessage = message;
      userData.timer = setTimeout(() => {
        usersMap.delete(message.author.id);
        console.log('Removed from RESET.');
      }, TIME);
      usersMap.set(message.author.id, userData);
    }
    else {
      ++msgCount;
      if(parseInt(msgCount) === LIMIT) {
        const role = message.guild.roles.cache.get('653342651739013197');
        message.member.roles.add(role);
        message.channel.send('You have been muted.');
        setTimeout(() => {
          message.member.roles.remove(role);
          message.channel.send('You have been unmuted');
        }, TIME);
      } else {
        userData.msgCount = msgCount;
        usersMap.set(message.author.id, userData);
      }
    }
  }
  else {
    let fn = setTimeout(() => {
      usersMap.delete(message.author.id);
      console.log('Removed from map.');
    }, TIME);
    usersMap.set(message.author.id, {
      msgCount: 1,
      lastMessage: message,
      timer: fn
    });
  }
});
    
    



bot.on('message', message=>{
    if(message.content === "שקד"){
        message.channel.send('ההומו');
    }
})

bot.on('message', message=>{
    if(message.content === "יבן זונה"){
        message.channel.send('אל תקלל יבן זונה');
    }
})

bot.on('message', message=>{
    if(message.content === "מעיין"){
        message.channel.send('הוא בן זונה');
    }
})

bot.on('message', message=>{
    if(message.content === "אלמוג"){
        message.channel.send('יש לו גדול');
    }
})

bot.on('message', message=>{
    if(message.content === "למה מי אתה"){
        message.channel.send('אבא שלך');
    }
})

bot.on('message', message=>{
    let args = message.content.substring(PREFIX.length).split(" ");

    switch(args[0]){
        case 'youtube':
        message.channel.send('https://www.youtube.com/')
        break;
     case 'clear':
            if(!args[1]) return message.channel.send('הכנס מספר הודעות שתרצה למחוק')
            message.channel.bulkDelete(args[1]);
            break;
            case 'info':
            const info = new Discord.MessageEmbed()
            .setTitle('מידע')
            .addField('owner name', message.author.username)
            .addField('version', version)
            .addField('server', message.guild.name)
            .setColor(0x0000FF)
            .setThumbnail(message.author.displayAvatarURL)
            .setFooter('bot by almog')
            message.channel.send(info);
        break;
        case 'help':
            const help = new Discord.MessageEmbed()
            .setTitle('עזרה')
            .addField('!youtube', 'הערוץ יוטיוב שלנו')
            .addField('!fivem', 'השרת פייבם שלנו')
            .addField('!teamspeake', 'לשרת טים ספיק שלנו')
            .setColor(0x0000FF)
            .setThumbnail(message.author.displayAvatarURL)
            .setFooter('bot by almog')
            message.channel.send(help);
        break;

        case 'kick':
            if(!message.member.roles.cache.find(r => r.name === "【⭐】Admin")) return message.channel.send('אין לך גישות לפעולה זו')


            if(!args[1]) message.channel.send('אתה צריך לתייג משתמש')

            const user = message.mentions.users.first();

            if(user){
                const member = message.guild.member(user);
                
                if (member) {
                    member.kick('אתה הוקקתה מהסרבר').then(() =>{
                        message.channel.send(`הבן זונה הוקק ${user.tag}`);
                    }).catch(err => {
                        message.channel.send('נתקלתי בבעיה איני יכולה להקיק את המשתמש');
                        console.log(err);
                    })
                } else{
                    message.channel.send("משתמש זה אינו בסרבר")
                } 
            
            }
       
        break;

    }
})
  

bot.login(token);.
