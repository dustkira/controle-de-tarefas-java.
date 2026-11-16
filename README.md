import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Main {
    private static final Scanner scanner = new Scanner(System.in);
    private static final List<Tarefa> tarefas = new ArrayList<>();
    private static int proximoId = 1;

    public static void main(String[] args) {
        int opcao;

        do {
            exibirMenu();
            opcao = lerInteiro("Escolha uma opção: ");

            switch (opcao) {
                case 1 -> listarTarefas();
                case 2 -> adicionarTarefa();
                case 3 -> concluirTarefa();
                case 4 -> removerTarefa();
                case 0 -> System.out.println("Até mais!");
                default -> System.out.println("Opção inválida. Tente novamente.");
            }
        } while (opcao != 0);

        scanner.close();
    }

    private static void exibirMenu() {
        System.out.println("\n=== CONTROLE DE TAREFAS ===");
        System.out.println("1. Listar tarefas");
        System.out.println("2. Adicionar tarefa");
        System.out.println("3. Concluir tarefa");
        System.out.println("4. Remover tarefa");
        System.out.println("0. Sair");
    }

    private static void listarTarefas() {
        if (tarefas.isEmpty()) {
            System.out.println("Nenhuma tarefa cadastrada.");
            return;
        }

        System.out.println("\n--- Minhas tarefas ---");
        for (Tarefa tarefa : tarefas) {
            System.out.println(tarefa);
        }
    }

    private static void adicionarTarefa() {
        System.out.print("Digite o título da tarefa: ");
        String titulo = scanner.nextLine().trim();

        if (titulo.isEmpty()) {
            System.out.println("O título não pode ficar vazio.");
            return;
        }

        tarefas.add(new Tarefa(proximoId++, titulo));
        System.out.println("Tarefa adicionada com sucesso!");
    }

    private static void concluirTarefa() {
        if (tarefas.isEmpty()) {
            System.out.println("Não há tarefas para concluir.");
            return;
        }

        listarTarefas();
        int id = lerInteiro("Digite o ID da tarefa concluída: ");

        for (Tarefa tarefa : tarefas) {
            if (tarefa.getId() == id) {
                tarefa.concluir();
                System.out.println("Tarefa marcada como concluída!");
                return;
            }
        }

        System.out.println("Não foi encontrada uma tarefa com esse ID.");
    }

    private static void removerTarefa() {
        if (tarefas.isEmpty()) {
            System.out.println("Não há tarefas para remover.");
            return;
        }

        listarTarefas();
        int id = lerInteiro("Digite o ID da tarefa que deseja remover: ");

        boolean removida = tarefas.removeIf(tarefa -> tarefa.getId() == id);
        if (removida) {
            System.out.println("Tarefa removida com sucesso!");
        } else {
            System.out.println("Não foi encontrada uma tarefa com esse ID.");
        }
    }

    private static int lerInteiro(String mensagem) {
        while (true) {
            System.out.print(mensagem);
            String entrada = scanner.nextLine();

            try {
                return Integer.parseInt(entrada.trim());
            } catch (NumberFormatException e) {
                System.out.println("Digite um número válido.");
            }
        }
    }

    private static class Tarefa {
        private final int id;
        private final String titulo;
        private boolean concluida;

        public Tarefa(int id, String titulo) {
            this.id = id;
            this.titulo = titulo;
            this.concluida = false;
        }

        public int getId() {
            return id;
        }

        public void concluir() {
            concluida = true;
        }

        @Override
        public String toString() {
            String status = concluida ? "Concluída" : "Pendente";
            return "[" + id + "] " + titulo + " — " + status;
        }
    }
}
