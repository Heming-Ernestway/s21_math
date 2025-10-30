CC_FLAGS = -Wall -Wextra -Werror -std=c11
CC_FLAGS_TESTS = -Wall -Wextra -Werror -std=c11 -I /usr/local/include

CHECK_FLAGS = -lcheck -lm -lpthread -lrt -lsubunit

all: s21_math.a test gcov_report

s21_math.a: s21_math.c 
	gcc $(CC_FLAGS) -c s21_math.c 
	ar rcs s21_math.a s21_math.o
	rm s21_math.o

s21_math: s21_math.c
	gcc $(CC_FLAGS_TESTS) s21_math.c -o s21_math s21_math.a

test: clean s21_math.c s21_test_math.c
	gcc $(CC_FLAGS_TESTS) s21_math.c s21_test_math.c tests/*.c -o unit_tests $(CHECK_FLAGS)

gcov_report: clean
	gcc $(CC_FLAGS_TESTS) s21_math.c s21_test_math.c tests/*.c -o gcov_tests -fprofile-arcs -ftest-coverage $(CHECK_FLAGS)
	./gcov_tests
	lcov -t "gcov_tests" -o gcov_tests.info -c -d .
	genhtml -o report gcov_tests.info
	open report/index.html

clean:
	rm -rf ./s21_math gcov_tests report ./unit_tests
	rm -f *.o *.gch s21_math.a
	rm -rf *.gcno *.gcda *.info

clang:
	clang-format -i {*.c,*.h} tests/*.c

clang_check:
	clang-format -n {*.c,*.h} tests/*.c

rebuild: clean all
