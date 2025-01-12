## VIII. Concorrência

### Escale através do processo modelo

Qualquer programa de computador, uma vez executado, está representado por um ou mais processos. Aplicações web têm tomado uma variedade de formas de processo de execução. Por exemplo, processos PHP rodam como processos filhos do Apache, iniciados sob demanda conforme necessário por volume de requisições. Processos Java tomam o caminho inverso, com a JVM proporcionando um processo uber maciço que reserva um grande bloco de recursos do sistema (CPU e memória) na inicialização, com concorrência gerenciada internamente via threads. Em ambos os casos, o(s) processo(os) em execução são apenas minimamente visível para os desenvolvedores da aplicação.

![Escala é expressado como processos em execução, a diversidade da carga de trabalho é expressada como tipos de processo.](/images/process-types.png)

**Na aplicação doze-fatores, processos são cidadãos de primeira classe.** Processos na aplicação doze-fatores utilizam fortes sugestões do modelo de processos UNIX para execução de serviços daemon, o desenvolvedor pode arquitetar a aplicação dele para lidar com diversas cargas de trabalho, atribuindo a cada tipo de trabalho a um _tipo de processo_. Por exemplo, solicitações HTTP podem ser manipuladas para um processo web, e tarefas background de longa duração podem ser manipuladas por um processo trabalhador.

Isto não exclui processos individuais da manipulação de sua própria multiplexação interna, por threads dentro do runtime da VM, ou o modelo async/evented encontrado em ferramentas como [EventMachine](https://github.com/eventmachine/eventmachine), [Twisted](http://twistedmatrix.com/trac/), ou [Node.js](http://nodejs.org/). Mas uma VM individual pode aumentar (escala vertical), de modo que a aplicação deve ser capaz de abranger processos em execução em várias máquinas físicas.

O modelo de processo realmente brilha quando chega a hora de escalar. O [compartilhar-nada, natureza horizontal particionada de um processo da aplicação doze-fatores](./processes) significa que a adição de mais simultaneidade é uma operação simples e de confiança. A matriz de tipos de processo e número de processos de cada tipo é conhecida como o _processo de formação_.

Processos de uma app doze-fatores [nunca deveriam daemonizar](https://dustin.sallings.org/2010/02/28/running-processes.html) ou escrever arquivos PID. Em vez disso, confiar no gerente de processo do sistema operacional (como [systemd](https://www.freedesktop.org/wiki/Software/systemd/), um gerenciador de processos distribuídos em uma plataforma de nuvem, ou uma ferramenta como [Foreman](http://blog.daviddollar.org/2011/05/06/introducing-foreman.html) em desenvolvimento) para gerenciar [fluxos de saída](./logs), responder a processos travados, e lidar com reinícios e desligamentos iniciados pelo usuário.
