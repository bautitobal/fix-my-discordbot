# fix-my-discordbot
 Hi, I'm doing a bot for discord, but I have three problems: The -embled simply does not work, and the -play command does not work either (to play music by writing the song or placing the link from a voice channel). I would be very supportive to help me solve the problem. Thank you!
         THE FILE:
         const Discord = require("discord.js");

const client = new Discord.Client();

const config = require ("./config.json")

const ytdl = require("ytdl-core");

const bot = new Discord.Client();

let prefix = config.prefix // config. del prefijo "-"

var version = '1.2';

var servers = {};

// Contestaciones
client.on("ready", () => {
    console.log(`Estoy listo!, 
             conectado en ${client.guilds.size} servidores y  ${client.users.size} usuarios.`,
             
             "Version: " + version);
 // JUGANDO A
             client.user.setPresence({
                status: "online",
                game: {
                    name: "-ayuda | Bot by Choripanycristi",
                    type: "PLAYING"
               
                }
                 });
                 
                })
                // chat automatico
 client.on("message", (message) => {
    if(message.content.startsWith("hola")) {    
    message.channel.send("Hola " + message.author.username + ", cómo estás?");
  
     // Nuevo miembro
     client.on("guildMemberAdd", (member) => {
        let canal = client.channels.get('599673358996733994'); 
        canal.send(`Hola ${member.user}, Bienvenido al Server  ${member.guild.name} pasala bien, bro!.`);
       
    });
    }
if(message.content.startsWith(prefix + "ayuda")) {
    message.channel.send("Enviando el mensaje");
if (!message.content.startsWith(prefix)) return; 
if (message.author.bot) return;
}
const embed = new Discord.RichEmbed() 
    .setTitle("Este es su título, puede contener 256 caracteres")
    .setAuthor(message.author.username, message.author.displayAvatarURL)
    .setColor(0x00AE86)
    .setDescription("Este es el cuerpo principal del texto, puede contener 2048 caracteres.")
    .setFooter("Pie de página, puede contener 1024 caracteres", client.user.avatarURL)
    .setImage(message.author.displayAvatarURL)
    .setThumbnail(message.author.displayAvatarURL)
    .setTimestamp()
    .setURL("https://www.twitch.tv/choripanycristi")
    .addField("Este es un título de campo", "Este es un valor de campo puede contener 1024 caracteres.")
    .addField("Campo en línea", "Debajo del campo en línea",  true)
    .addBlankField(true)
    .addField("Campo en línea 3", "Puede tener un máximo de 25 campos.", true);
    if (!message.content.startsWith(prefix)) return; 
if (message.author.bot) return;
});
// Conectarse a canal 
bot.on('message' , message => {
    let args =message.content.substring(prefix.length).split(" ");

    switch (args[0]) {
        case 'play':
            function play(connection, message){
                var server = servers[message.guild.id];

                server.dispatcher = connection.playStream(ytdl(server.queue[0], {filter: "audioonly"}));
                
                server.queue.shift();

                server.dispatcher.on("end", function(){
                    if(server.queue[0]){
                        play(connection, message);  
                    }else {
                        connection.disconnect();
                    }
                });

            
            
            
            }


            if(!args[1]){
                message.channel.send("¡Necesitas enviar un link!");
                return;
            }

            if(!message.member.voiceChannel){
                    message.channel.send("Debes estar en un canal de voz. ;) ");
                    return;
             } 

             if(!servers[message.guild.id]) servers[message.guild.id] = {
                queue: []
             }

             var server = servers[message.guild.id];

             server.queue.push(args[1]);  

             if(!message.guild.voiceConnection) message.member.voiceChannel.join().then(function(connection){
                play(connection, message); 
             })
            
            
            
            
             break;
        }


        
    });
//  DON'T TOUCH

client.login(config.token);
    
