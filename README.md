# parallel-programming


<!--


python -m venv .venv
source .venv/bin/activate # Linux/Mac
.venv\Scripts\activate # Windows


/////////

pip install -r requirements.txt

-->

<p><b>UNICAP - 2026.1</b></p>

<p>Projeto desenvolvido para a disciplina de <b>Programação Paralela e Distribuída.</b></p>

<p> <b>Alunos:</b> Matheus Veríssimo, Maria Luiza Ribeiro, Rafael Sampaio, Vinícius Martins. </p>
<br>

<p> <b>Resumo do projeto:</b>
Esse projeto aborda o desafio de encriptar grandes volumes de dados com eficiência,
cenário comum em servidores de cloud e sistemas de backup. O algoritmo escolhido
é o AES, padrão global de encriptação, implementado no modo CTR (Counter Mode).
A motivação para esse modo específico é técnica: o AES puro é uma cifra de bloco, e
paralelizar blocos encadenados comprometeria a segurança da encriptação. O modo
CTR transforma o AES em uma cifra de fluxo, onde cada bloco utiliza apenas seu
próprio contador como entrada, eliminando dependências entre blocos e permitindo
paralelismo total sem abrir mão da segurança. É esperado ganhos expressivos com
CUDA para arquivos grandes, identificando o ponto onde o paralelismo em GPU
passa a compensar em relação à CPU. O grupo alinhou também o risco de
complexidade, onde nesse caso pivotaríamos para outro algoritmo de criptografia. </p>
