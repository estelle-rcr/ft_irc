# Ft_IRC

## The project
This project aims to build an Internet Relay Chat server using sockets in cpp. The server handles mutliple connections from clients (using either netcat - a TCP requests handler - or IRSSI - an IRC client).

Connections are handled simultaneously and are non-blocking which means the server is always available and treat all the clients' responses continously.

In bonus, we created a bot as a service client connecting to our server and answering other client's requests through the server.

## How to use it
The project runs only on a Linux distribution.
  Compile the mandatory project:
  `make`

  Compile the bonus version:
  `make bonus` or `make ircbot` (to make only the bot exec)

  Start the program - server side:
  `./ircserver <PORT> <PASSWORD>`
    `./ircserver 6667 pwd`

  Start the program - client and bot side:
   `nc -C localhost 6667` - then complete the user/service registration (pass + nick + user commands)
   `IRSSI` to start our reference's client and then `/connect localhost 6667 pwd` (for auto-connect)
   `./ircbot localhost pwd [PORT]` to start the bot (default port is 6667)

## Functionalities

According to the IRC reference document (RFC 2810 to 2813), we implemented the following commands:

### Registration/connection
- PASS	(ex: `PASS pwd`)
- NICK	(ex: `NICK user`)
- USER	(ex: `USER username 0 * :user fullname`)
- SERVICE	(ex: `SERVICE bot * *42paris.fr 0 0 :My bot offers this service...` )
- QUIT	(ex: `QUIT [message]`)

### User commands
- MODE	(ex: `MODE user +r` - available modes: B, i, o, r, s)
- OPER	(ex: `OPER user pwd` - host must be registered in the conf files)
- KILL  (ex: `KILL otheruser comment`)
- WHO
- WHOIS	(ex: `WHOIS otheruser`)

### Channel commands
- JOIN	(ex: `JOIN #channel`)
- PART	(ex: `PART #channel`)
- INVITE	(ex: `INVITE otheruser #channel`)
- KICK	(ex: `KICK #channel otheruser`)
- MODE	(ex: `MODE #42 +k keypass` - to setup a keypass to the channel - modes available: b, i, k, o)
- TOPIC (ex: `TOPIC #channel`)
- NAMES	(ex: `NAMES #channel`)
- LIST  (ex: `LIST`or `LIST #channel`)

### Message commands
- PRIVMSG	(ex: `PRIVMSG otheruser :full message` or `PRIVMSG #channel :full message` - use /MSG in IRSSI)
- NOTICE	(same use as PRIVMSG, but no feedback message is sent)


### Server commands
- DIE (or press `CTRL-C` to stop the server)
- INFO
- MOTD
- PING
- PONG
- TIME
- VERSION

## Bonus
### File transfer
File transfer is handled in direct client-to-client transfer when using the client IRSSI. Our server enables the transfer by transfering the sending and accepting/refusing requests and file locations (ports) in PRIVMSG.

- Use: `/dcc send <otheruser> <"Path/filename">` to send a file
`/dcc get <otheruser> <"filename">` to accept the file
`/dcc close <get/send> <otheruser>` to refuse the file

### The bot
The bot is a small client auto-connecting to our server using the SERVICE registration protocol. Once connected with the server, it receives the PRIVMSG from users through the server and can answer to TIME and QUOTES upon user's requests.

- Use: `./ircbot <HOSTNAME> <PASSWORD> [PORT]`
 `./ircbot localhost pwd`
On user side, request as: `PRIVMSG Impostor QUOTE` or `PRIVMSG Impostor TIME`

## Resources
- RFC 2810 on Architecture: https://datatracker.ietf.org/doc/html/rfc2810
- RFC 2811 on Channel Management: https://datatracker.ietf.org/doc/html/rfc2811
- RFC 2812 on Client procotol: https://datatracker.ietf.org/doc/html/rfc2812
- RFC 2813 on Server Procotol: https://datatracker.ietf.org/doc/html/rfc2813
- Understanding the server-client communication in TCP: http://www.lsv.fr/~rodrigue/teach/npp/2012/tp1.pdf
- A tutorial on how to setup of a server and a client in TCP and UDP: http://vidalc.chez.com/lf/socket.html
- Using poll instead of select: https://www.ibm.com/docs/en/i/7.4?topic=designs-using-poll-instead-select
- IRC protocol examples : http://chi.cs.uchicago.edu/chirc/irc_examples.html
