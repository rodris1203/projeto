use std::io;

#[derive(Debug)]
struct Usuario {
    nome: String,
    email: String,
    idade: u8,
    ativo: bool,
}

impl Usuario {
    // Criação com validação
    fn novo(nome: String, email: String, idade: u8) -> Result<Usuario, String> {
        if nome.trim().is_empty() {
            return Err("❌ Nome não pode estar vazio.".into());
        }

        if !email.contains('@') || !email.contains('.') {
            return Err("❌ Email inválido.".into());
        }

        if idade == 0 || idade > 150 {
            return Err("❌ Idade deve estar entre 1 e 150.".into());
        }

        Ok(Usuario {
            nome,
            email,
            idade,
            ativo: true,
        })
    }

    fn exibir_perfil(&self) {
        println!("\n--- ✅ Perfil do Usuário ---");
        println!("Nome  : {}", self.nome);
        println!("Email : {}", self.email);
        println!("Idade : {}", self.idade);
        println!("Status: {}", if self.ativo { "Ativo" } else { "Inativo" });
    }

    fn desativar(&mut self) {
        self.ativo = false;
    }
}

fn main() {
    let mut entrada = String::new();

    // Nome
    println!("Digite o nome:");
    io::stdin().read_line(&mut entrada).expect("Erro ao ler nome");
    let nome = entrada.trim().to_string();
    entrada.clear();

    // Email
    println!("Digite o email:");
    io::stdin().read_line(&mut entrada).expect("Erro ao ler email");
    let email = entrada.trim().to_string();
    entrada.clear();

    // Idade
    println!("Digite a idade:");
    io::stdin().read_line(&mut entrada).expect("Erro ao ler idade");
    let idade: u8 = match entrada.trim().parse() {
        Ok(valor) => valor,
        Err(_) => {
            println!("❌ Idade inválida (precisa ser um número entre 1 e 150)");
            return;
        }
    };
    entrada.clear();

    // Criação e validação
    match Usuario::novo(nome, email, idade) {
        Ok(mut usuario) => {
            usuario.exibir_perfil();

            // Testando desativar
            usuario.desativar();
            println!("\n🔒 Usuário desativado:");
            usuario.exibir_perfil();
        }
        Err(erro) => println!("Erro ao criar usuário: {}", erro),
    }
}
