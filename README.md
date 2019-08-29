This is a connect 4 AI I made freshman year in highschool. This is programed in racket. To play it, run it and then type "(main start-state)" in the command line.

The source code does not appear well on git so here is it in text form:

(require 2htdp/image)
(require 2htdp/universe)

;; do not modify the constants
(define RED 1) 
(define BLACK 2)
(define BLANK 0)
(define ROWS 10)  
(define COLUMNS 10)



;;Eval constants
(define DEFAULT-DEPTH 3) 
(define WEIGHT3ROW 100)
(define WEIGHT2ROW 5)
(define WEIGHTOFDISTANCE .0001)
(define WEIGHT3ROWOPPONENT 40)
(define WEIGHT2ROWOPPONENT 2)
(define WEIGHTOFDISTANCEOPPONENT .00005)
(define WEIGHTOFWIN +inf.0)


;;natural recursion temp
#;

(define (fn-for-list lst)
  (cond [(empty? lst) ...]
        [else
         ...(first lst) (fn-for-list (rest lst))]))
;;mutual recursion w/ acc
#;
(define (fn-for-lot lst)
  (cond [(empty? lst) ...]
        [else
         ...(fn-for-thing lst) (fn-for-lot (rest lst))]))
#;
(define (fn-for-thing thing)
  (... thing (fn-for-lot ...)))
;; generative recursion
#;
(define (fn-for-generative-recursion obj )
  (if (trivial?)
      trivial-case
      (generate-next-case obj)))
          
;;template for world-state
#;
(define (fn-for-ws ws)
  (... (make-worldstate position whose-turn settings other-info)))



;;We believe these moves are best because red will try to instantly win and black will try to block it.
(check-expect (computer-moves (make-world-state test-board2 RED false false)) (make-world-state
                                                                               (list
                                                                                (list 0 0 0 0 0 0 0 0 0 0)
                                                                                (list 0 0 0 0 0 0 0 0 0 0)
                                                                                (list 0 0 0 0 0 0 0 0 0 0)
                                                                                (list 0 0 0 0 0 0 0 0 0 0)
                                                                                (list 0 0 0 0 0 0 0 0 0 0)
                                                                                (list 0 0 0 0 0 0 1 1 1 1)
                                                                                (list 0 0 0 0 0 0 0 0 0 0)
                                                                                (list 0 0 0 0 0 0 0 0 0 0)
                                                                                (list 0 0 0 0 0 0 0 0 0 0)
                                                                                (list 0 0 0 0 0 0 0 0 0 0))
                                                                               2
                                                                               #false
                                                                               #false))
(check-expect (computer-moves (make-world-state test-board2 BLACK false false)) (make-world-state
                                                                                 (list
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 2 1 1 1)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0))
                                                                                 1
                                                                                 #false
                                                                                 #false))

;;We believe these values are correct because black should try to block the diagnol and red should try to win.
(check-expect (computer-moves (make-world-state test-board3 RED false false)) (make-world-state
                                                                               (list
                                                                                (list 0 0 0 0 0 0 0 0 0 0)
                                                                                (list 0 0 0 0 0 0 0 0 0 0)
                                                                                (list 0 0 0 0 0 0 0 0 0 0)
                                                                                (list 0 0 0 0 0 0 1 1 2 1)
                                                                                (list 0 0 0 0 0 0 0 1 2 2)
                                                                                (list 0 0 0 0 0 0 0 0 1 2)
                                                                                (list 0 0 0 0 0 0 0 0 0 1)
                                                                                (list 0 0 0 0 0 0 0 0 0 0)
                                                                                (list 0 0 0 0 0 0 0 0 0 0)
                                                                                (list 0 0 0 0 0 0 0 0 0 0))
                                                                               2
                                                                               #false
                                                                               #false))
(check-expect (computer-moves (make-world-state test-board3 BLACK false false)) (make-world-state
                                                                                 (list
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 2 1 2 1)
                                                                                  (list 0 0 0 0 0 0 0 1 2 2)
                                                                                  (list 0 0 0 0 0 0 0 0 1 2)
                                                                                  (list 0 0 0 0 0 0 0 0 0 1)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0))
                                                                                 1
                                                                                 #false
                                                                                 #false))

;;We believe these values are correct because it priortizes winning over blocking a potential win. 
(check-expect (computer-moves (make-world-state test-board4 RED false false)) (make-world-state
                                                                               (list
                                                                                (list 0 0 0 0 0 0 0 0 0 0)
                                                                                (list 0 0 0 0 0 0 0 0 0 2)
                                                                                (list 0 0 0 0 0 0 0 0 2 1)
                                                                                (list 0 0 0 0 0 0 1 1 2 1)
                                                                                (list 0 0 0 0 0 0 0 1 2 2)
                                                                                (list 0 0 0 0 0 0 0 0 1 2)
                                                                                (list 0 0 0 0 0 0 0 0 0 1)
                                                                                (list 0 0 0 0 0 0 0 0 0 0)
                                                                                (list 0 0 0 0 0 0 0 0 0 0)
                                                                                (list 0 0 0 0 0 0 0 0 0 0))
                                                                               2
                                                                               #false
                                                                               #false))
(check-expect (computer-moves (make-world-state test-board4 BLACK false false)) (make-world-state
                                                                                 (list
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 2 2)
                                                                                  (list 0 0 0 0 0 0 0 0 2 1)
                                                                                  (list 0 0 0 0 0 0 0 1 2 1)
                                                                                  (list 0 0 0 0 0 0 0 1 2 2)
                                                                                  (list 0 0 0 0 0 0 0 0 1 2)
                                                                                  (list 0 0 0 0 0 0 0 0 0 1)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0))
                                                                                 1
                                                                                 #false
                                                                                 #false))
;;Red should try to setup the double trap for victory, which it succesfully does. 
(check-expect (computer-moves (make-world-state test-board5 RED false false)) (make-world-state
                                                                                 (list
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 2)
                                                                                  (list 0 0 0 0 0 0 1 2 2 1)
                                                                                  (list 0 0 0 0 0 0 0 1 2 1)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0)
                                                                                  (list 0 0 0 0 0 0 0 0 0 1)
                                                                                  (list 0 0 0 0 0 0 0 0 0 0))
                                                                                 2
                                                                                 #false
                                                                                 #false))



;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; Some parts of this function can not get code coverage
; because the the layer being evaluated depends on whether
; or not the depth is odd or even. There is no way to fix
; this with just check-expects since we have to use a
; pre-defined constant.
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

;; state --> state
;; uses a min max algorithm to determine the best move for the next player. This works for both red and black.
 
(define (computer-moves state)
    (local [(define (minimax-of-board state acc max?)
            (if max?
                (if (check-win? state) WEIGHTOFWIN
                    (if (> acc 0)
                        (minimax-of-lob (map (lambda (x) (make-hypothetical-move state x)) (legal-next-moves state)) (- acc 1) false)
                        (- (evaluation-function-complex state))))

                (if (check-win? state) (- WEIGHTOFWIN)
                    (if (> acc 0)
                        (minimax-of-lob (map (lambda (x) (make-hypothetical-move state x)) (legal-next-moves state)) (- acc 1) true)
                        (evaluation-function-complex state)))))

          (define (minimax-of-lob los acc max?)
            (if max?
                (if (empty? los) (- WEIGHTOFWIN)
                    (max (minimax-of-board (first los) acc true) (minimax-of-lob (rest los) acc true)))
                
                (if (empty? los) WEIGHTOFWIN
                    (min (minimax-of-board (first los) acc false) (minimax-of-lob (rest los) acc false)))))]


    (make-move state
               (argmax
                (lambda (x) (minimax-of-board (make-hypothetical-move state x) (- DEFAULT-DEPTH 1) true))
                (legal-next-moves state)))))

;; state --> Number
;; you will implement this function to determine the value of a state (terminal or non-terminal)

;;This test was purely to ensure that our functions were working correctly as it encoporated one of each possible situation.
(check-expect (evaluation-function-complex (make-world-state test-board1 RED false false)) 633.00005)
(check-expect (evaluation-function-complex (make-world-state test-board1 BLACK false false)) -80.99965)

;;This test checks the value of an earlygame board, and it shows that the evaluation function
;;knows its a bad state because black has 3 in a row, but it isn't an unwinnable position because
;;black still has counterplay. 
(check-expect (evaluation-function-complex (make-world-state test-board2 RED false false)) 105.0006)
(check-expect (evaluation-function-complex (make-world-state test-board2 BLACK false false)) -42.0003)

;;This shows that red knows getting a 3 in a row diagnolly is good and that black knows it's not good.
(check-expect (evaluation-function-complex (make-world-state test-board3 RED false false)) 113.00035)
(check-expect (evaluation-function-complex (make-world-state test-board3 BLACK false false)) -40.99965)

;;This shows that both sides know that having access to a 3 in a row is good. 
(check-expect (evaluation-function-complex (make-world-state test-board4 RED false false)) 73.00035)
(check-expect (evaluation-function-complex (make-world-state test-board4 BLACK false false)) 59.00035)




;;We believe these values are correct because our simple evaluation just values a board based on proximity to center.
(check-expect (evaluation-function-simple (make-world-state test-board1 RED false false)) -.0002)
(check-expect (evaluation-function-simple (make-world-state test-board1 BLACK false false)) .0002)
(check-expect (evaluation-function-simple (make-world-state test-board2 RED false false)) .0006)
(check-expect (evaluation-function-simple (make-world-state test-board2 BLACK false false)) -.0006)
(check-expect (evaluation-function-simple (make-world-state test-board3 RED false false)) 0)
(check-expect (evaluation-function-simple (make-world-state test-board3 BLACK false false)) 0)
(check-expect (evaluation-function-simple (make-world-state test-board4 RED false false)) 0)
(check-expect (evaluation-function-simple (make-world-state test-board4 BLACK false false)) 0)
(check-expect (evaluation-function-simple (make-world-state test-board5 RED false false)) -.0001)
(check-expect (evaluation-function-simple (make-world-state test-board5 BLACK false false)) .0001)



(define (evaluation-function-simple ws)
  (local [(define position (world-state-position ws))
          (define p1 (world-state-whose-turn ws))
          (define p2 (if (= RED p1) BLACK RED))]
    (-  (* (distancefromcenter p1  position) WEIGHTOFDISTANCE)
        (* (distancefromcenter p2 position) WEIGHTOFDISTANCE))))

(define (evaluation-function-complex ws)
  (local [(define position (world-state-position ws))
          (define p1 (world-state-whose-turn ws))
          (define p2 (if (= RED p1) BLACK RED))]
    ( - (+ (* (num3row p1 position) WEIGHT3ROW)
           (* (num2row p1 position) WEIGHT2ROW)
           (* (distancefromcenter p1 position) WEIGHTOFDISTANCE))
        (+ (* (num3row p2 position) WEIGHT3ROWOPPONENT)
           (* (num2row p2 position) WEIGHT2ROWOPPONENT)
           (* (distancefromcenter p2 position) WEIGHTOFDISTANCEOPPONENT)
           ))))



(define blank-board (make-list COLUMNS
                               (make-list ROWS BLANK)))
(define red-board (make-list COLUMNS
                             (make-list ROWS RED)))
(define black-board (make-list COLUMNS
                               (make-list ROWS RED)))
(define test-board1 (list
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 1 0 1 0 0 0 0 0 0)
                     (list 0 0 1 0 0 0 0 0 0 0)
                     (list 0 1 0 1 2 0 0 0 0 0)
                     (list 0 0 0 0 0 2 0 0 0 0)
                     (list 0 0 0 0 0 0 2 0 0 0)
                     (list 1 0 0 0 0 0 0 0 0 0)
                     (list 1 0 0 0 1 0 0 0 0 0)
                     (list 1 0 1 0 2 2 0 0 0 0)
                     (list 0 0 1 0 1 1 1 0 0 0)))

(define test-board2 (list
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 0 0 0 0 0 0 1 1 1)
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 0 0 0 0 0 0 0 0 0)))

(define test-board3 (list
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 0 0 0 0 0 0 1 2 1)
                     (list 0 0 0 0 0 0 0 1 2 2)
                     (list 0 0 0 0 0 0 0 0 1 2)
                     (list 0 0 0 0 0 0 0 0 0 1)
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 0 0 0 0 0 0 0 0 0)))

(define test-board4 (list
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 0 0 0 0 0 0 0 0 2)
                     (list 0 0 0 0 0 0 0 0 2 1)
                     (list 0 0 0 0 0 0 0 1 2 1)
                     (list 0 0 0 0 0 0 0 1 2 2)
                     (list 0 0 0 0 0 0 0 0 1 2)
                     (list 0 0 0 0 0 0 0 0 0 1)
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 0 0 0 0 0 0 0 0 0)
                     (list 0 0 0 0 0 0 0 0 0 0)))
(define test-board5  (list
                      (list 0 0 0 0 0 0 0 0 0 0)
                      (list 0 0 0 0 0 0 0 0 0 0)
                      (list 0 0 0 0 0 0 0 0 0 0)
                      (list 0 0 0 0 0 0 0 0 0 0)
                      (list 0 0 0 0 0 0 0 0 0 2)
                      (list 0 0 0 0 0 0 1 2 2 1)
                      (list 0 0 0 0 0 0 0 1 2 1)
                      (list 0 0 0 0 0 0 0 0 0 0)
                      (list 0 0 0 0 0 0 0 0 0 0)
                      (list 0 0 0 0 0 0 0 0 0 0)))
  




;; you must implement the above two functions as part of the asignment, but may create additional
;; helper functions


(define-struct world-state (position whose-turn settings other-info))
;; position is what checkers are on the board
;; whose-turn is the player who is about to move
;; settings and other-info are future functionality


;; returns the checker color (RED, BLACK, or BLANK) of the specified position
;; on the board
(define (piece-at board row column)
  (get-nth row (get-nth column board)))


;; Natural List --> Element
;; returns the nth element of a list.  for former LISP programmers learning Racket :-)
(define (get-nth n alist)
  (list-ref alist n))


(check-expect (check-for-each-place test-board1 RED (lambda (b co r c) (if (= co (list-ref (list-ref b c) r)) 1 0))) 14)
(check-expect (check-for-each-place test-board1 BLACK (lambda (b co r c) (if (= co (list-ref (list-ref b c) r)) 1 0))) 5)  

;;Signature: board color (fn board color row colums -> Natural) -> Natural
;;Runs a function on each piece in the board and sums the results.
 
(define (check-for-each-place board color fn)
  (local [(define (check-rows row columns)
            (if (= row (- ROWS 1))
                (if (= columns (- COLUMNS 1))
                    (fn board color row columns)
                    (+ (fn board color row columns) (check-rows 0 (+ columns 1))))
                (+ (fn board color row columns) (check-rows (+ row 1) columns))))]
    (check-rows 0 0)))

(check-expect (num3row RED test-board1) 7)
(check-expect (num3row RED test-board2) 1)
(check-expect (num3row RED test-board3) 1)

;; Signature Board -> natural
;; returns the number of 3 in a rows in the board 
(define (num3row color board)
  (local [(define (up-column-open? board color x y)
            (if (< y 3)
                0
                (local [(define column (get-nth x board))]
                  (if (and (= (get-nth (- y 3) column) BLANK)
                           (=(get-nth y column)
                             (get-nth  (- y 1) column)
                             (get-nth  (- y 2) column)
                             color))
                      1
                      0))))
          
          (define (right-row-open? board color x y)
            (if (or (>= x (- COLUMNS 2)) (= x 0))
                0
                (if (and (= (get-nth y (get-nth (- x 1) board)) BLANK)
                         (= (get-nth y (get-nth x board))
                            (get-nth y (get-nth (+ 1 x) board))
                            (get-nth y (get-nth (+ 2 x) board))
                            color))
                    1
                    0)))
          (define (left-row-open? board color x y)
            (if (or (< x 2) (= x (- COLUMNS 1)))
                0
                (if (and (= (get-nth y (get-nth (+ x 1) board)) BLANK)
                         (= (get-nth y (get-nth x board))
                            (get-nth y (get-nth (- x 1) board))
                            (get-nth y (get-nth (- x 2) board))
                            color))
                    1
                    0)))
          
          (define (down-left-open? board color x y)
            (if (or (> y (- ROWS 3)) (= y 0)
                    (< x 2) (= x (- COLUMNS 1)))
                0
                (if (and (= (get-nth (- y 1) (get-nth ( + x 1) board)) BLANK)
                         (= (get-nth y (get-nth x board))
                            (get-nth (+ y 1) (get-nth ( - x 1) board))
                            (get-nth (+ y 2) (get-nth ( - x 2) board))
                            color))
                    1
                    0)))
          (define (up-right-open? board color x y)
            (if (or (< y 2) (= y (- ROWS 1))
                    (>= x (- COLUMNS 2)) (= x 0))
                0
                (if (and (= (get-nth (+ y 1) (get-nth ( - x 1) board)) BLANK)
                         (= (get-nth y (get-nth x board))
                            (get-nth (- y 1) (get-nth ( + x 1) board))
                            (get-nth (- y 2) (get-nth ( + x 2) board))
                            color))
                    1
                    0)))
          
          (define (down-right-open? board color x y)
            (if (or (>= y (- ROWS 2)) (= y 0)
                    (>= x (- COLUMNS 2)) (= x 0))
                0

                (if (and (= (get-nth (- y 1) (get-nth ( - x 1) board)) BLANK)
                         (= (get-nth y (get-nth x board))
                            (get-nth (+ y 1) (get-nth ( + x 1) board))
                            (get-nth (+ y 2) (get-nth ( + x 2) board))
                            color))
                    1
                    0)))
          (define (up-left-open? board color x y)
            (if (or (< y 2) (= y (- ROWS 1))
                    (< x 2) (= x (- COLUMNS 1)))
                0

                (if (and (= (get-nth (+ y 1) (get-nth ( + x 1) board)) BLANK)
                         (= (get-nth y (get-nth x board))
                            (get-nth (- y 1) (get-nth ( - x 1) board))
                            (get-nth (- y 2) (get-nth ( - x 2) board))
                            color))
                    1
                    0)))]
    (+ (check-for-each-place board color up-column-open?)
       (check-for-each-place board color right-row-open?)
       (check-for-each-place board color left-row-open?)
       (check-for-each-place board color up-right-open?)
       (check-for-each-place board color down-left-open?)
       (check-for-each-place board color down-right-open?)
       (check-for-each-place board color up-left-open?))))




(check-expect (num2row RED test-board1) 3)
(check-expect (num2row RED test-board2) 1)
(check-expect (num2row RED test-board3) 3)

;; Signature Board -> natural
;; returns the number of 2 in a rows in the board 
(define (num2row color board)
  (local [(define (up-column-open? board color x y)
            (if (< y 2)
                0
                (local [(define column (get-nth x board))]
                  (if (and (= (get-nth (- y 2) column) BLANK)
                           (=(get-nth y column)
                             (get-nth  (- y 1) column)
                             color))
                      1
                      0))))
          
          (define (right-row-open? board color x y)
            (if (or (>= x (- COLUMNS 1)) (<= x 1))
                0
                (if (and (= (get-nth y (get-nth (- x 1) board)) BLANK)
                         (or (= (get-nth y (get-nth (- x 2) board)) BLANK)
                             (= (get-nth y (get-nth (- x 2) board)) color))
                            
                         (= (get-nth y (get-nth x board))
                            (get-nth y (get-nth (+ 1 x) board))
                            color))
                    1
                    0)))
          (define (left-row-open? board color x y)
            (if (or (< x 1) (>= x (- COLUMNS 2)))
                0
                (if (and (= (get-nth y (get-nth (+ x 1) board)) BLANK)
                         (or (= (get-nth y (get-nth (+ x 2) board)) BLANK)
                             (= (get-nth y (get-nth (+ x 2) board)) BLANK))
                            
                         (= (get-nth y (get-nth x board))
                            (get-nth y (get-nth (- x 1) board))
                            color))
                    1
                    0)))
          (define (both-row-open? board color x y)
            (if (or (< x 2) (>= x (- COLUMNS 1)))
                0
                (if (and (= (get-nth y (get-nth (+ x 1) board))
                            (get-nth y (get-nth (- x 2) board))
                            BLANK)
                         (= (get-nth y (get-nth x board))
                            (get-nth y (get-nth (- x 1) board))
                            color))
                    1
                    0)))]
          
          
          
    (+ (check-for-each-place board color up-column-open?)
       (check-for-each-place board color right-row-open?)
       (check-for-each-place board color left-row-open?)
       (check-for-each-place board color both-row-open?))))



(check-expect (distancefromcenter RED test-board1) 3)
(check-expect (distancefromcenter RED test-board2) 6)
(check-expect (distancefromcenter RED test-board3) 7)

;; Signature Board -> Natural
;; Returns the value of the board position, which is proportional to the distance from the center of the board.
;; This is mainly used as a tiebreaker

(define (distancefromcenter color board)
  (check-for-each-place board color (lambda (b co r c)
                                      (if (and (= co (list-ref (list-ref b c) r)) (> c 2) (< c (- COLUMNS 3)))
                                          (min (- c 2) (- COLUMNS c 3))  ;;Assigns values based on closeness to center of board.
                                          0))))




    
(define (main state)
  (local 
    [(define board 
       (make-list COLUMNS
                  (make-list ROWS 0)))
     
     (define PIECE-SIZE 30)
     
     (define RED-CHECKER (circle PIECE-SIZE "solid" "red"))
     (define BLACK-CHECKER (circle PIECE-SIZE "solid" "black"))
     (define BLANK-CHECKER (circle PIECE-SIZE "solid" "white"))
     
     (define OFFSET (/ PIECE-SIZE .66))
     (define WIDTH
       (+ (* COLUMNS 2.5 PIECE-SIZE) (* 0.5 PIECE-SIZE)))
     (define HEIGHT
       (+ (* ROWS 2.5 PIECE-SIZE) (* 0.5 PIECE-SIZE)))
     
     (define MTS 
       (rectangle WIDTH HEIGHT "solid" "yellow"))
     (define (place-checker state x y mouse-event)
       (local
         [(define move (map-coordinates x y))
          (define next-state (make-move state move))]
         (cond
           [(and (string=? mouse-event "button-down")
                 (member move (legal-next-moves state)))
            (if (check-win? next-state)  
                (cond
                  [(= (world-state-whose-turn state) RED)
                   ;(make-world-state red-board RED "" "")]
                   "RED WINS"]
                  [(= (world-state-whose-turn state) BLACK)
                   ;(make-world-state black-board BLACK "" "")])
                   "BLACK WINS"])
                (local [(define result (computer-moves next-state))]
                  (if (check-win? result)
                      (cond
                        [(= (world-state-whose-turn next-state) RED)
                         "RED WINS"]
                        ;(make-world-state red-board RED "" "")]
                        [(= (world-state-whose-turn next-state) BLACK)
                         ;(make-world-state black-board BLACK "" "")])
                         "BLACK WINS"])
                      result)))]
           [else state])))
     (define (display-column2 column x-offset y-offset image)
       x-offset)
     (define (display-column column x-offset y-offset image)
       (cond
         [(empty? column) image]
         [else
          (place-image
           (cond 
             [(= (first column) RED) RED-CHECKER]
             [(= (first column) BLACK) BLACK-CHECKER]
             [(= (first column) BLANK) BLANK-CHECKER])
           x-offset y-offset 
           (display-column (rest column) x-offset (+ y-offset (* 2.5 PIECE-SIZE)) image))]))
     
     (define (display-board-helper position x-offset image)
       (cond 
         [(empty? position) image]
         [else
          (display-board-helper
           (rest position)
           (+ x-offset (* 2.5 PIECE-SIZE))
           (display-column (first position)
                           x-offset
                           OFFSET image))]))
     
     (define (display-board position)
       (display-board-helper position OFFSET MTS))
     (define (render state)
       (display-board (world-state-position state)))
     
     (define (map-coordinate lower upper click pos)
       (cond
         [(and (> click lower) (< click upper)) pos]
         [(> pos (max ROWS COLUMNS)) -1]
         [else
          (map-coordinate (+ lower (* 2.5 PIECE-SIZE)) (+ upper (* 2.5 PIECE-SIZE)) click (+ 1 pos))]))
     
     (define (map-coordinates x y) 
       (list (map-coordinate (/ PIECE-SIZE 2) (+  (/ PIECE-SIZE 2) (* 2 PIECE-SIZE)) x 0)
             (map-coordinate (/ PIECE-SIZE 2) (+  (/ PIECE-SIZE 2) (* 2 PIECE-SIZE)) y 0)))]
    
    (big-bang state 
              (on-mouse place-checker) 
              (to-draw render))))

;; *** this function permits you to make both legal and illegal moves
;; *** you do not need to use this function and probably should not.  someone thought of a reason
;; *** for it to exist and so i included it.  to be clear, your program is only permitted to 
;; *** make legal moves.
(define (make-hypothetical-move state move)
  (local [(define (update-column turn column current move)
            (cond
              [(empty? column) empty]
              [else
               (cons
                (cond
                  [(= current move)
                   turn]
                  [else (first column)])
                (update-column turn (rest column) (+ 1 current) move))]))
          
          (define (do-move board turn move-x move-y current-x)
            (cond
              [(empty? board) empty]
              [else
               (cons
                (cond
                  [(= move-x current-x) (update-column turn (first board) 0 move-y)]
                  [else (first board)])
                (do-move (rest board) turn move-x move-y (+ 1 current-x)))]))]
    (make-world-state
     (do-move (world-state-position state)
              (world-state-whose-turn state) 
              (first move) (second move) 0)
     (cond
       [(= (world-state-whose-turn state) RED) BLACK]
       [(= (world-state-whose-turn state) BLACK) RED])
     (world-state-settings state)
     (world-state-other-info state))))

;; you will use this function.  it takes as input the move you will make, represented as a list of X Y coordinates
(define (make-move state move)
  (if (member move (legal-next-moves state))
      (make-hypothetical-move state move)
      state))

;; world-state --> list
;; returns all of the legal moves for the current position
(define (legal-next-moves state)
  (local [
          (define (first-blank column pos)
            (cond
              [(empty? column) (- pos 1)]
              [(not (= (first column) BLANK))
               (- pos 1)]
              [else (first-blank (rest column) (+ 1 pos))]))
          (define (get-moves board-state column)
            (cond
              [(empty? board-state) empty]
              [else
               (local [(define blank (first-blank (first board-state) 0))]
                 (if (< blank 0)
                     (get-moves (rest board-state) (+ 1 column))
                     (cons
                      (list column (first-blank (first board-state) 0))
                      (get-moves (rest board-state) (+ 1 column)))))]))]
    (get-moves (world-state-position state)
               0)))


;; check-win:  world-state --> boolean
;; determines whether the game has ended with a victory for whoever just moved
(define (check-win? state)
  (local [
          ;; !!! should go back and fix these functions with piece-at function
          (define (up-column board color x y)
            (if (< y 3)
                false
                (local [(define column (get-nth x board))]
                  (= (get-nth  (- y 1) column)
                     (get-nth  (- y 2) column)
                     (get-nth  (- y 3) column)
                     color))))
          
          (define (right-row board color x y)
            (if (>= x (- COLUMNS 3))
                false
                (= (get-nth y (get-nth (+ 1 x) board))
                   (get-nth y (get-nth (+ 2 x) board))
                   (get-nth y (get-nth (+ 3 x) board))
                   color)))
          
          (define (up-right board color x y)
            (if (or (< y 3)
                    (>= x (- COLUMNS 3)))
                false
                (= (get-nth (- y 1) (get-nth ( + x 1) board))
                   (get-nth (- y 2) (get-nth ( + x 2) board))
                   (get-nth (- y 3) (get-nth ( + x 3) board))
                   color)))
          
          (define (down-right board color x y)
            (if (or (>= y (- ROWS 3))
                    (>= x (- COLUMNS 3)))
                false
                (= (get-nth (+ y 1) (get-nth ( + x 1) board))
                   (get-nth (+ y 2) (get-nth ( + x 2) board))
                   (get-nth (+ y 3) (get-nth ( + x 3) board))
                   color)))
          
          (define (victory? board x y)
            (let
                ([color (get-nth y (get-nth x board))])
              (if (= color BLANK)
                  false
                  (or
                   (up-column board color x y)
                   (right-row board color x y)
                   (up-right board color x y)
                   (down-right board color x y)))))
          
          (define (walk-column board col row)
            (cond
              [(= row ROWS) false]
              [else
               (or
                (victory? board col row)
                (walk-column board col (+ 1 row)))]))
          
          (define (walk-board board col)
            (cond
              [(= col COLUMNS) false]
              [else
               (or (walk-column board col 0)
                   (walk-board board (+ 1 col)))]))]
    (walk-board (world-state-position state) 0)))


(define START-BOARD
  (make-list COLUMNS
             (make-list ROWS BLANK)))
(define start-state
  (make-world-state START-BOARD RED 5 empty))

(define test-board
  (list
   (list 1 2 3 4 5 6 7 8 9)
   (list 11 12 13 14 15 16 17 18 19)
   (list 21 22 23 24 25 26 27 28 29)
   (list 31 32 33 34 35 36 37 38 39)
   (list 0 0 0 0 0 0 0 0 0)
   (list 0 0 0 0 0 0 0 0 0)
   (list 0 0 0 0 0 0 0 0 0)
   (list 0 0 0 0 0 0 0 0 0)))

(define test-state (make-world-state test-board RED 5 empty))


; 1.Given a 5 second budget we can search with depth 4 with our simple evaluation function and search with depth 3
; with our complex evaluation function. The complex evaluation is definitely stronger than the simple evaluation, even
; considering the fact that it only searches 10% of what the simple eval searches.
; 2. The complex evaluation does very well, the only way to beat it is to guarantee a win before it guarantees a win on you. Since
; it can only check depth level 3 in 5 seconds a skilled player should be able to beat it, but it should be able to consitantly beat an
; amateur player. So far we have not had anyone beat the complex evaluation at depth level 4, however due to large amount of time it takes
; to evaluate each move we do not have a large sample size to work with.
