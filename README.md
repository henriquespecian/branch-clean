# Limpar git

O problema é sempre o mesmo, um monte de branch que não é usada nem no ambiente local nem no remoto

## Local

O comando executa uma limpeza de branchs mergeadas com uma branch específica ou com a master, é só adicionar ele como um alias no .gitconfig e executar

	#Todas as branchs locais mergeadas com uma branch específica (caso seja inserida) ou com a master
	localClean = "!f() { \
					git fetch --prune; \
					echo branchs já mergeadas com ${1-master} que serão deletadas:; \
					git branch --merged ${1-master} | egrep -v \"(master|release|qa|dev)\"; \
					if [ ${2-no} == 'delete' ]; then \
						git branch --merged ${1-master} | egrep -v \"(master|release|qa|dev)\" | xargs git branch -d; \
					fi \
				}; f"
	
Em um primeiro momento ele lista as branchs que serão excluidas

  	localClean <nome-da-branch>
        
Depois é só adicionar **delete** na frente do comando e ele vai remover **localmente** as branchs que foram listadas como mergeadas com uma branch específica

  	localClean <nome-da-branch> delete
  
  
  
  ## Remoto
        
O funcionamento é o mesmo para o comando de exclusão da origin

	#Todas as branchs remotas mergeadas com uma branch específica (caso seja inserido) ou com a master
	#OBS: Caso delete alguma coisa que não deveria, esse comando não delete a branch local
	originClean = "!f() { \
					git fetch --prune; \
					echo branchs já mergeadas com ${1-master} que serão deletadas:; \
					git branch -a --merged ${1-master} | egrep -v \"(master|release|qa|dev)\"; \
					if [ ${2-no} == 'delete' ]; then \
						git branch -a --merged ${1-master} | egrep -v \"(master|release|qa|dev)\" | xargs -n 1 git push --delete origin; \
					fi \
				}; f"

Para um dry run

  	originClean <nome-da-branch>
    
Para a exclusão das branchs na **origin** (use com sabedoria)

  	originClean <nome-da-branch> delete
        

        
        

