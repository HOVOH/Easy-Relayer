# How to easily run a Relayer Node
One obstacle in getting Relayer as the main provider of off-chain worker is to make sure that the network is strong and has a lot of workers, but running a Relayer was not an easy task for the everyday crypto enthusiast as it needed programming knowledge to create a bot and make sure it stays up and running reliably. So we did all the job for you and wrapped that in a Docker image to make it almost a set and forget robot working relentlessly for you. But before that, you will also need to bond your wallet with the Relayer contract on the website https://relay3r.network/. You can bond 0 tokens, but some jobs will need a minimum amount of RLR bonded to execute. There is also a bonus on job reward when you bond 200 RLR or greater. Once you bonded, you will need to wait until you can activate your wallet, activate it and then you can start doing jobs.

So what’s Docker you might ask? It’s basically emulating a machine with all the required programs and files in what we call an image. You don’t really need to know more to run a Relayer node, but if you want to learn there are tons of resources available online.
Get started by installing Docker:
-	Docker installation guide for Windows: https://docs.docker.com/docker-for-windows/install/
-	Docker installation guide for Linux: https://docs.docker.com/engine/install/.You will also need to install docker-compose: https://docs.docker.com/compose/install/

Once it’s done, we will need to create the configuration files. You can either download them from GitHub at https://github.com/HOVOH/Easy-Relayer/tree/relayer, or you can create them manually, it’s only two files.
If you decide to create them manually, create a folder somewhere in your pc to store the files. Then create a file called `docker-compose.yml` and paste the following content in the file:
```
version: "3.3"
services:
  relayer:
    image: docker.pkg.github.com/relay3r-network/relay3r-jobs/node:new-combined
    restart: always
    volumes:
      - ".env:/relayer/.env"
```
Then create a `.env` file in the same folder and paste the following content in it:
```
MNEMONIC=
INFURA_PROJECT_ID=
JOBS=
API_CALL_LIMIT=100000
```
The `.env` file is the where you will set your information the Relayer will use. As you can see you have 4 basic things to fill, so let’s start working on that.

In order for the node to communicate with the Ethereum blockchain, we will need to have access to a full Ethereum node. Running an Ethereum node is a big task as you must download the entire blockchain history which is about 800GB to this date. To make our life easy, we will use [Infura](https://infura.io/) as our provider so we don’t have to run an Eth node. Their basic package is free and let you do 100000 request a day. The `API_CALL_LIMIT` key in the `.env` is what it refers to. If you upgrade your package or use another provider you can set you api call limit per day and the Relayer will make sure to run every job as often as it can while respecting the limit. We will need a project id to authenticate on Infura’s server. So register on Infura, then go to create a new project, give it a name and copy you Infura project ID in the `.env` file.

Now let’s select the jobs we want to run by going to [Relayer's repo](https://github.com/relay3r-network/relay3r-jobs), under the “Available Jobs” section are all the jobs you can currently run. You will want to concatenate all the jobs with a comma (no space) and set that string in the `.env` for the JOBS key. For example, let’s say I want to run the GetBackETHRelayer job and the UnitradeRelayer Job, the JOBS line has to have `JOBS=GetBackETHRelayer,UnitradeRelayer`.

Last thing: your wallet. We higly recommend you create a new wallet for your Relayer as it is not recommended to have your mnemonic which let you recover the wallet in plain text on your PC. The best way to do this is create a new wallet and send some funds to it. But if you want to use a pre-existing wallet go ahead and set your on the `MNEMONIC=` in the `.env` file. If you want to create a new wallet, leave that field empty and it will be populated automatically with the wallet mnemonic of startup.

That’s it, you are ready to execute your first jobs. Just open a command prompt in the folder you created with the `docker-compose.yml` and `.env` file and type the command `docker-compose up -d`. If you did not set a MNEMONIC in the `.env`, the field will get populated automatically, so make sure to note that somewhere else just in case. You can also use it to import that wallet in Metamask or any other wallet app.

The current Docker set up is set up in a way where it always restarts when Docker starts. 

You might want to monitor what your Relayer is doing. If you are on windows you can simply open the Docker Desktop app and you will be able to see the Docker instance there and monitor the log file and see if it’s up. If you are on Linux or like the command prompt you can always go to your docker folder and do `docker-compose logs -f -–tail=50` to see the output of the Relayer, or `docker-compose ps` to see it’s status and uptime. 
