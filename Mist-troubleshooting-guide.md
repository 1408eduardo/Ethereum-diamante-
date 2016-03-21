# Wallet troubleshooting

Please note that the Wallet is still in beta and problems can be expected. This guide will help you troubleshooting some of the most common problems. Some steps require the use of the command line or the developer console that Mist offers. If you need assistance you can ask for help on https://gitter.im/ethereum/mist.

First step is to make sure you have the latest version of the Wallet. The Wallet is under active development and new versions contain bugfixes. You can download the latest version here: https://github.com/ethereum/mist/releases.

The next step is to see if your problem is a common problem:
- ["Your computers time it out of sync!" error](#your-computers-time-it-out-of-sync-error)
- [Unable to find peers](#unable-to-find-peers)
- [The wallet is synchronized but is stuck during the last part](#the-wallet-is-synchronized-but-is-stuck-during-the-last-part)
- [My transaction is not confirmed](#my-transaction-is-not-confirmed)
- [Unable to import pre-sale wallet](#unable-to-import-presale-wallet)

If its not a common problem you can look at log files to see if there are errors. It is important to known that the wallet consists of two applications. Mist, which runs the wallet and an ethereum node which connects to the network. Communication between the two is done over ipc. A quick scan can be done to determine if the problem is within the Wallet or within the node. The last part of the node log file can be viewed from Mist through the top menu bar -> develop -> Show log file. If it contains an error try to search for it on https://github.com/ethereum/mist/issues?utf8=%E2%9C%93&q=is%3Aissue to see if its already been report and if someone offered a solution.

If there are no errors the next step is to see if there are problems within Mist. Open the console via the top menu -> develop -> Toggle developer tools -> Wallet UI and look for errors (ignore the "Failed to load resource: net::ERR_FILE_NOT_FOUND" error) and look on the github issue list for simular problems. If there are none type `web3.eth.blockNumber` on the console and see if you get the latest block number. If you cannot find the problem create an issue on github and describe as good as possible what you have done and which error you got, preferable with a screenshot and log files included.

## "Your computers time it out of sync!" error
If the time of your computers is deviated the wallet isn't able to connect with the network. The wallet is able to detect the clock offset and informs the user of this problem. Please enable time synchronisation:
- Windows: http://windows.microsoft.com/en-gb/windows/set-clock#1TC=windows-7
- OSX: https://support.apple.com/kb/PH21911?locale=en_US

## Unable to find peers
There are several reasons why the Wallet is not able to connect to the network.
- time synchronization problems, please update to the latest version of the wallet
- a firewall is preventing access, check the logs of your firewall.

In some cases users have had succes by starting geth manually. Stop the wallet if its still running and open a command prompt on Windows or a terminal on OSX or Linux. Navigate to the location where you have unpacked the Wallet. You should see a resource/node sub directory with geth inside. Navigate to that directory and give the following command:

Windows: `.\geth.exe --fast --cache 512 --nat=none`
OSX/Linux: `./geth --fast --cache 512 --nat=none`

You can optionally increase the log level by adding `--verbosity 5`.

## The wallet is synchronized but is stuck during the last part
Each time the Wallet is started it needs to sync the chain with the network. The first time it needs to download and verify the entire chain which can take a long time. Therefore a smarter and faster solution is used the first time the Wallet is started. It will not download and verify the entire chain but will only download specific parts up to a specific block (latest block - 1024 blocks). After that it will download and process all data which can take quite some time. As a result it looks like the Wallet is almost synchronized but the final blocks take a very long time. You can find more details here: https://github.com/ethereum/go-ethereum/pull/1889.

## My transaction is not confirmed
There are multiple causes:
- You lowered the transaction fee. Miners have the option to ignore transaction with a too low fee. It can take a long time before your transaction is included in the block chain. Currently Mist doesn't provide a solution to resend the transaction with a higher transaction fee.
- Mist is not fully synchronized with the network. Make sure you have peers (see the top bar) and the block number in Mist matches the one that is shown on http://etherscan.io. Also check if the block number increases over time. If not, the node is not able to connect to the network. See the section "X hours since last block".
- If you have peers and your block number matches or differs only a couple of blocks to the one reported on http://etherscan.io click on the unconfirmed transaction. A popup will open which shows the transaction details and the transaction hash. The transaction hash is actually a link to etherscan. Click on this link. If etherscan shows the transaction details this means the transaction is processed and part of the blockchain, just wait before the wallet receives the block it is included. Otherwise the transaction has not been processed (yet). It is a bit harder to determine where your transaction actually is. It might have never been send onto the network.

## Unable to import presale wallet
Due to a bug importing a presale wallet can fail. It is possible to import the presale wallet manually following these instructions: https://github.com/ethereum/go-ethereum/wiki/Managing-your-accounts#importing-your-presale-wallet