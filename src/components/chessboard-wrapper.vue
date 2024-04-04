<!--The draggable pieces attribute doesn't work with a value of true or false.-->
<!--It's presence alone makes pieces draggable.-->
<!--Adding the attribute with javascript is a workaround.-->
<!--<chess-board :orientation="orientation" :draggable-pieces="draggablePieces"></chess-board>-->
<template>
  <chess-board :orientation="orientation" :draggable-pieces="draggablePieces"></chess-board>
</template>

<script>
import 'chessboard-element';
import { Chess } from "chess.js";
import {getNextMove} from "@/services/chessMoveService";
let board, game, randomMoveInterval;

export default {
  name: "chess-board-wrapper",
  data() {
    return {
      orientation: "white",
      draggablePieces: undefined
    }
  },
  methods: {
    async makeRandomMove() {
      let possibleMoves = game.moves();

      // game over
      if (!game.isGameOver() && possibleMoves.length === 0) {
        clearInterval(randomMoveInterval);
        return;
      }
      const nextMove = await getNextMove({gameFen: game.fen()});
      const [from, to] = Object.entries(nextMove)[0];
      game.move({
        from: from.toLowerCase(),
        to: to.toLowerCase(),
      });

      board.setPosition(game.fen());
    },
    addEventListeners() {
      board.addEventListener('drag-start', (e) => {
        // eslint-disable-next-line no-unused-vars
        const {source, piece, position, orientation} = e.detail;

        // do not pick up pieces if the game is over
        if (game.isGameOver()) {
          e.preventDefault();
          return;
        }

        // only pick up pieces for White
        if (piece.search(/^b/) !== -1) {
          e.preventDefault();
          return;
        }
      });
      board.addEventListener('drop', (e) => {
        const {source, target, setAction} = e.detail;

        // see if the move is legal
        const move = game.move({
          from: source,
          to: target,
          promotion: 'q'// NOTE: always promote to a queen for example simplicity
        });

        // illegal move
        if (move === null) {
          setAction('snapback');
          return;
        }

        // make random legal move for black
        window.setTimeout(this.makeRandomMove, 250);
      });
      // update the board position after the piece snap
      // for castling, en passant, pawn promotion
      board.addEventListener('snap-end', () => {
        board.setPosition(game.fen(), false);
      });
    },
    quickStartGame() {
      board = this.$el;
      game = new Chess();
      board.start(false);
      this.draggablePieces = true;
      this.addEventListeners();

    },
    // eslint-disable-next-line no-unused-vars
    advancedConfigStartGame(color, fen, selfPlay) {
      board = this.$el;
      game = new Chess();

      const  { validate_fen: validateFen } = game;
      let result = validateFen(fen);

      if (fen && result.valid) {
        board.setPosition(fen, false);
        game.load(fen);
      } else {
        board.start(false);
      }
      this.orientation = color;
      if (selfPlay) {
        randomMoveInterval = window.setInterval(this.makeRandomMove, 500);
      } else {
        this.draggablePieces = true;
        this.addEventListeners();
        if (game.turn() === 'b') {
          window.setTimeout(this.makeRandomMove, 500);
        }
      }
    }
  }
}
</script>

<style scoped>

</style>
