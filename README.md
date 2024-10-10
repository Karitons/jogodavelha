package jogodavelha;

import java.util.Random;
import java.util.Scanner;

public class jogodavelha {

    public static void main(String[] args) {
        menu();
    }

    public static void menu() {
        String[][] tabela = new String[3][3];
        iniciarTabuleiro(tabela);
        int jogada, nJog = 1;

        do {
            Scanner scan = new Scanner(System.in);
            exibirTabuleiro(tabela);
            System.out.println("Números de jogadas: " + nJog);
            System.out.println("Digite o nº da casa (1-9) ou 999 para sair ou 123 para reiniciar:");
            jogada = scan.nextInt();

            switch (jogada) {
                case 1: case 2: case 3: case 4: case 5: case 6: case 7: case 8: case 9:
                    int posL = (jogada - 1) / 3;
                    int posC = (jogada - 1) % 3;
                    if (verifJogo(tabela, posL, posC)) {
                        tabela[posL][posC] = "O"; // Marca a jogada do jogador
                        nJog++;

                        if (venceu(tabela, "O")) {
                            exibirTabuleiro(tabela);
                            System.out.println("Você venceu!");
                            return;
                        }

                        if (nJog == 10) {
                            System.out.println("O jogo empatou!");
                            return;
                        }

                        // Turno do computador
                        turnoPC(tabela);
                        nJog++;

                        if (venceu(tabela, "X")) {
                            exibirTabuleiro(tabela);
                            System.out.println("O computador venceu!");
                            return;
                        }
                    }
                    break;

                case 999:
                    System.out.println("Finalizando o jogo...");
                    return;

                case 123:
                    System.out.println("Reiniciando o jogo...");
                    iniciarTabuleiro(tabela);
                    nJog = 1;
                    break;

                default:
                    System.out.println("Jogada inválida");
                    break;
            }
        } while (true);
    }

    public static void iniciarTabuleiro(String[][] tabela) {
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                tabela[i][j] = String.valueOf(i * 3 + j + 1); // Inicializa com números de 1 a 9
            }
        }
    }

    public static void exibirTabuleiro(String[][] tabela) {
        System.out.println("Jogo da Velha");
        System.out.println("| " + tabela[0][0] + " | " + tabela[0][1] + " | " + tabela[0][2] + " |");
        System.out.println("| " + tabela[1][0] + " | " + tabela[1][1] + " | " + tabela[1][2] + " |");
        System.out.println("| " + tabela[2][0] + " | " + tabela[2][1] + " | " + tabela[2][2] + " |");
        System.out.println();
    }

    public static boolean verifJogo(String[][] tabela, int posL, int posC) {
        return tabela[posL][posC].equals(String.valueOf((posL * 3 + posC + 1))); // Verifica se a casa está disponível
    }

    public static void turnoPC(String[][] tabela) {
        Random random = new Random();
        int posL, posC;

        do {
            posL = random.nextInt(3);
            posC = random.nextInt(3);
        } while (!verifJogo(tabela, posL, posC)); // Procura uma casa disponível

        tabela[posL][posC] = "X"; // Marca a jogada do computador
    }

    public static boolean venceu(String[][] tabela, String jogador) {
        // Verifica linhas, colunas e diagonais
        for (int i = 0; i < 3; i++) {
            if ((tabela[i][0].equals(jogador) && tabela[i][1].equals(jogador) && tabela[i][2].equals(jogador)) || 
                (tabela[0][i].equals(jogador) && tabela[1][i].equals(jogador) && tabela[2][i].equals(jogador))) {
                return true;
            }
        }
        return (tabela[0][0].equals(jogador) && tabela[1][1].equals(jogador) && tabela[2][2].equals(jogador)) ||
               (tabela[0][2].equals(jogador) && tabela[1][1].equals(jogador) && tabela[2][0].equals(jogador));
    }
}
