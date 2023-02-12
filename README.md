# HomeWork2
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.*;
import java.util.function.Function;
import java.util.stream.Collectors;

public class classHomeWork2 {

/**
 * Задание:
 * 1. Создайте вложенный класс LoginValidationException, унаследуйте его от Exception
 * 2. Реализуйте проверку "Логина" в методе validateLogin по следуюзим правилам:
 * - должен содержать только латинские буквы, цифры и знак подчеркивания
 * - должен содержать как минимум одну маленькую, одну большую буквы и нижнее подчеркивание
 * - максимальная длинна логина- 20 символов
 * - если логин не соответствует требованиям - выбросить LoginValidationException
 * - можно использовать регулярные выражения
 * 3. Реализуйте проверку логина в методе isLoginValid по следующим правилам
 * - метод должен вызывать метод validateLogin
 * - если метод validateLogin не выбросил ошибку - вернуть true
 * - если метод validateLogin выбросил ошибку - вернуть false
 */

public static List<String> loginList = Arrays.asList(
        "Minecraft_12", // true 1
        "Player_3433", // true 2
        "Dok_a111", // true 3
        "Java", // false 4
        "1122233", // false 5
        "Play__", // false 6
        "_Sun2_", // true 7
        "____", // false 8
        "Winx!", // false 9
        "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa12_", // false 10
        "WOWOWOOWOWOWOOWOWOWOWOWOW", // false 11
        "Correct_22"                    // true 12
        );

public static List<Boolean> checkLoginResults = Arrays.asList(
        true, true, true, false, false, false, true, false, false, false, false, true
        );

public static void main(String[] args) {
        System.out.println("\nTests for validateLogin");
        AntiCheat.run();
        for (int i = 0; i < loginList.size(); i++) {
        try {
        validateLogin(loginList.get(i));
        printTestCase(i, checkLoginResults.get(i), true, 20);
        } catch(Exception e) {
        printTestCase(i, checkLoginResults.get(i), false, 20);
        }
        }

        System.out.println("\nTests for isLoginValid");
        AntiCheat.run();
        for (int i = 0; i < loginList.size(); i++)
        printTestCase(i + loginList.size(),
        checkLoginResults.get(i),
        isLoginValid(loginList.get(i)),
        20);
        }

        public static class LoginValidationException extends Exception{
            Exception LoginValidationException = new Exception();


        }

public static boolean validateLogin(String login)  {
    Exception LoginValidationException = new LoginValidationException();
    String specialChars = "~`!@#$%^&*()-_=+\\|[{]};:'\",<.>/?";
    char currentCharacter;
    boolean lowerCasePresent = false; // Чисто return заполнить
    String str = login;

        if (login.length()>20){
            throw  new  RuntimeException(LoginValidationException);

        }else {
            int z =login.length()-1;
        boolean numberPresent = false;
        boolean upperCasePresent = false;
        boolean specialCharacterPresent = false;
        for (int i = 0; i < str.length(); i++) {
            //
            currentCharacter = str.charAt(i);
            if (Character.isDigit(currentCharacter)) {
                numberPresent = true;
            }
            if (Character.isUpperCase(currentCharacter)) {
                upperCasePresent = true;
            }
            if (specialChars.contains(String.valueOf(currentCharacter))) {
                specialCharacterPresent = true;
            }
            if ((numberPresent & upperCasePresent & specialCharacterPresent)){
                break;
            }
            if (((numberPresent)==false||(upperCasePresent)==false||(specialCharacterPresent)==false)&&(i==z)){
                 throw  new  RuntimeException(LoginValidationException);

            }
        }}
    return lowerCasePresent;
}
public static Boolean isLoginValid(String login)  {
try {
    validateLogin(login);
}catch (RuntimeException e ){
    return false;
}
    return true;
}

public static  class AntiCheat {
public static void run() {
        StringBuilder sb = new StringBuilder("");
        List<String> antiCheatList = new ArrayList<>();
        antiCheatList.addAll(loginList);
        boolean b = antiCheatList.addAll(checkLoginResults.stream().map(Object::toString).collect(Collectors.toList()));
        antiCheatList.add(sb.toString());
        calcHash(antiCheatList);
        };

public  static String bytesToHex(byte[] bytes) {
        char[] HEX_ARRAY = "0123456789ABCDEF".toCharArray();
        char[] hexChars = new char[bytes.length * 2];
        for (int j = 0; j < bytes.length; j++) {
        int v = bytes[j] & 0xFF;
        hexChars[j * 2] = HEX_ARRAY[v >>> 4];
        hexChars[j * 2 + 1] = HEX_ARRAY[v & 0x0F];
        }
        return new String(hexChars);
        }

public static void calcHash(List<String> list) {
        String total = String.join("", list);
        try {
        MessageDigest md = MessageDigest.getInstance("MD5");
        md.update(total.getBytes());
        byte[] digest = md.digest();
        System.out.println("AntiCheatCheck: " + bytesToHex(digest));
        } catch (NoSuchAlgorithmException ignored) {}
        }
        }

public static String constLen(String str, int len) {
        StringBuilder sb = new StringBuilder(str);
        while (len-- - str.length() > 0) sb.append(" ");
        return sb.toString();
        }

public static void printTestCase(int n, Boolean exp, Boolean act, int minLen) {
        Function<String, String> green = str -> "\u001B[34m" + str + "\u001B[0m";
        Function<String, String> yellow = str -> "\u001B[33m" + str + "\u001B[0m";
        System.out.print( "TEST CASE " + constLen(String.valueOf(n), 4));
        System.out.print( "Ожидание: " + yellow.apply(constLen(exp.toString(), minLen)) + " Реальность: " + green.apply(constLen(act.toString(), minLen) + " "));
        if (Objects.equals(exp, act)) System.out.print("✅"); else System.out.print("❌");
        System.out.println();
        }

        }
