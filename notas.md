Comecou por qualquercoisa do robin

robin
-> Prova
->gera Criptograma com a chave publica do caso
-> assina com privada do user
-> junta o certificado ed25519 e numero da caso
-> cifra tudo com AES-GCM-256

Manda para o wiregard
o wiregard faz:
	- faz a troca das chaves para o tunneling
	- ou seja cifra envia e decifra

chega ao sistema
-> descifra o wire gard, 
-> decifra com a chave aes
-> autentica com o certificado Ed
->  verifica assinatura
-> atribui certificado mTLS
-> trocar o numero do caso pelo UID do caso e atribuir ao hashpointer (block)