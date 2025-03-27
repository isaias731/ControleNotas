import java.util.Scanner;

public class ControleNotas {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String resposta;

        do {
            System.out.print("Quantos alunos deseja cadastrar? ");
            int numAlunos = scanner.nextInt();
            scanner.nextLine(); // Limpar o buffer

            String[] nomesAlunos = new String[numAlunos];
            double[][] notasAlunos = new double[numAlunos][2];
            double[] mediasAlunos = new double[numAlunos];
            String[] statusAlunos = new String[numAlunos];

            for (int i = 0; i < numAlunos; i++) {
                System.out.println("\nAluno " + (i + 1) + ":");
                System.out.print("Nome: ");
                nomesAlunos[i] = scanner.nextLine();

                for (int j = 0; j < 2; j++) {
                    double nota;
                    while (true) {
                        System.out.print("Nota " + (j + 1) + ": ");
                        nota = scanner.nextDouble();
                        if (nota >= 0 && nota <= 10) {
                            notasAlunos[i][j] = nota;
                            break;
                        } else {
                            System.out.println("Nota inválida. Digite uma nota entre 0 e 10.");
                        }
                    }
                    scanner.nextLine(); // Limpar o buffer
                }

                mediasAlunos[i] = (notasAlunos[i][0] + notasAlunos[i][1]) / 2.0;
                statusAlunos[i] = calcularStatus(mediasAlunos[i]);
            }

            exibirResultados(nomesAlunos, mediasAlunos, statusAlunos, notasAlunos);

            System.out.print("\nDeseja cadastrar outra turma? (S/N): ");
            resposta = scanner.nextLine();

        } while (resposta.equalsIgnoreCase("S"));

        System.out.println("Programa encerrado.");
        scanner.close();
    }

    public static String calcularStatus(double media) {
        if (media >= 7) {
            return "Aprovado";
        } else if (media >= 5) {
            return "Recuperação";
        } else {
            return "Reprovado";
        }
    }

    public static void exibirResultados(String[] nomes, double[] medias, String[] status, double[][] notas) {
        System.out.println("\nResultados:");
        double somaMedias = 0;
        double maiorNota = notas[0][0];
        double menorNota = notas[0][0];
        int aprovados = 0, recuperacao = 0, reprovados = 0;

        for (int i = 0; i < nomes.length; i++) {
            System.out.println(nomes[i] + " - Média: " + medias[i] + " - Status: " + status[i]);
            somaMedias += medias[i];

            for (int j = 0; j < 2; j++) {
                if (notas[i][j] > maiorNota) {
                    maiorNota = notas[i][j];
                }
                if (notas[i][j] < menorNota) {
                    menorNota = notas[i][j];
                }
            }

            if (status[i].equals("Aprovado")) aprovados++;
            else if (status[i].equals("Recuperação")) recuperacao++;
            else reprovados++;
        }

        System.out.println("\nMédia da turma: " + (somaMedias / nomes.length));
        System.out.println("Maior nota registrada: " + maiorNota);
        System.out.println("Menor nota registrada: " + menorNota);
        System.out.println("Total de aprovados: " + aprovados);
        System.out.println("Total em recuperação: " + recuperacao);
        System.out.println("Total de reprovados: " + reprovados);
    }
}
