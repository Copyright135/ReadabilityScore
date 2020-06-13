package readability;

import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        File file = new File(args[0]);

        double wordCount = 0;
        double sentenceCount = 0;
        double characterCount = 0;
        double syllableCount = 0;
        double polysyllables = 0;

        String regex = "[.!?]+[\\s\\pZ]*";


        /*
         * Scan provided data and gather data on words, sentences, characters, and syllables
         */
        try (Scanner fileScanner = new Scanner(file)) {
            while (fileScanner.hasNext()) {
                String line = fileScanner.nextLine();

                sentenceCount += line.split(regex).length;
                characterCount += line.replaceAll("\\s", "").length();
                String[] words = line.split("[.!?,]*[\\s\\pZ]");
                wordCount += words.length;

                for (String s : words) {
                    double syllables = getSyllables(s);
                    syllableCount += syllables;
                    if (syllables > 2) {
                        polysyllables++;
                    }

                }
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }


        /*
         * Output all of the collected data
         */
        System.out.println("Words: " + wordCount);
        System.out.println("Sentences: " + sentenceCount);
        System.out.println("Characters: " + characterCount);
        System.out.println("Syllables: " + syllableCount);
        System.out.println("Polysyllables: " + polysyllables);

        System.out.print("Enter the score you want to calculate (ARI, FK, SMOG, CL, all):");
        String arg;
        try (Scanner scan = new Scanner(System.in)) {
            arg = scan.nextLine();
        }
        System.out.println();
        System.out.println();

        double[] data = new double[] {wordCount, sentenceCount, characterCount, syllableCount, polysyllables};

        processCommand(arg, data);
    }

    /*
     * Handles which method to pass the data to
     * Returns the score in double format
     */
    private static double processCommand(String arg, double[] data) {
        String str = "";
        double score = 0;
        switch (arg.toUpperCase()) {
            case "ARI":
                score = calcAri(data[0], data[1], data[2]);
                str = "Automated Readability Index: " + score +  " (about " + getAgeGroup(score) + " year olds).";
                break;
            case "FK":
                score = calcFk(data[0], data[1], data[3]);
                str = "Flesch–Kincaid readability tests: " + score +  " (about " + getAgeGroup(score) + " year olds).";
                break;
            case "SMOG":
                score = calcSmog(data[4], data[1]);
                str = "Simple Measure of Gobbledygook: " + score +  " (about " + getAgeGroup(score) + " year olds).";
                break;
            case "CL":
                score = calcCl(data[0], data[1], data[2]);
                str = "Coleman–Liau index: " + score +  " (about " + getAgeGroup(score) + " year olds).";
                break;
            case "ALL":
                double age = (getAgeGroup(processCommand("ARI", data))
                        + getAgeGroup(processCommand("FK", data))
                        + getAgeGroup(processCommand("SMOG", data))
                        + getAgeGroup(processCommand("CL", data))) / 4.0;
                str = "This text should be understood in average by " + age + " year olds.";
                break;
            default:
                break;
        }
        System.out.println(str);
        return score;
    }


    /*
     * The following methods are the different readability calculations
     * Automated Readability, Flesch-Kincaid, Simple Measure of Gobbledygook, and Coleman-Liau
     */
    private static double calcAri(double wordCount, double sentenceCount, double characterCount) {
        return 4.71 * (characterCount / wordCount) + 0.5 * (wordCount / sentenceCount) - 21.43;
    }

    private static double calcFk(double wordCount, double sentenceCount, double syllableCount) {
        return 0.39 * (wordCount / sentenceCount) + 11.8 * (syllableCount / wordCount) - 15.50;
    }

    private static double calcSmog(double polysllables, double sentenceCount) {
        return 1.043 * (Math.sqrt(polysllables * (30 / sentenceCount))) + 3.1291;
    }

    private static double calcCl(double wordCount, double sentenceCount, double characterCount) {
        return 0.0588 * ((characterCount / wordCount) * 100) - 0.296 * ((sentenceCount / wordCount) * 100) - 15.8;
    }


    /*
     * Counts the amount of syllables present in each word for use in some of the indexes
     */
    private static double getSyllables(String s) {
        double syllables = 0;
        String regex = "[aeiouyAEIOUY]";

        for (int i = 0; i < s.length() - 1; i++) {
            if (i == s.length() - 1 && s.substring(i).matches("[aiouyAIOUY]")) {
                syllables++;
            } else if (s.substring(i, i + 1).matches(regex) && !(s.substring(i + 1, i + 2).matches(regex))) {
                syllables++;
            }
        }

        return syllables < 1 ? 1 : syllables;
    }


    /*
     * Returns the age related to each score.
     */
    private static int getAgeGroup(double score) {
        int[] ageGroups = new int[] {6, 7, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 24, 24};

        if (score > 14) {
            score = 14;
        } else if (score < 1) {
            score = 1;
        }

        int roundedScore = (int) Math.ceil(score);
        return ageGroups[roundedScore - 1];
    }
}
