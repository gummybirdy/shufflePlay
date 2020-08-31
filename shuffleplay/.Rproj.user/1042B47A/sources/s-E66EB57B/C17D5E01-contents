#' Shuffle Play Generator
#'
#' \code{shufflePlay} takes an input of song IDs and simulates a shuffled playlist that corresponds to an intuitive understanding of randomness.
#' @param songs a strictly positive vector of numeric song IDs.
#' @param n the number of songs to play.
#' @param randomness a non-negtive number affecting how much of the playlist will shuffle. See 'Details'.
#' @param buffer the number of songs that must play before a song repeats.
#' @param minRec the minimum proportion of songs to recycle into.
#' @param verbose if TRUE, also returns every song played, in order.
#' @details
#' \code{shufflePlay} uses a truncated form of true randomness, described internally as a recycle bin, to generate playlists. The size of the bin is determined partially by the following equation: \deqn{1 - e^{-randomness * ln(length(songs))}}
#' And partially by the parameters \code{buffer} and \code{minRec}, as described. The bin occupies the latter portion of the playlist of songs. Once played, songs are shuffled into the bin via a uniform random variable.
#' @return
#' \code{shufflePlay} returns a list containing 1-2 elements:
#' \tabular{ll}{
#'   \code{plays} \tab a vector of the number of times each song played. \cr
#'   \code{playlist} \tab if \code{verbose=TRUE}, a vector of every song played, in order. \cr
#' }
#' @author Andrew Morse, \email{andrewmorse@@uchicago.edu}
#' @examples
#' # generates the plays and playlist object for a list of 200 songs, played 300 times
#' shufflePlay(1:200, n=300, verbose=T)

shufflePlay <- function(songs, n=100, randomness=0.05, buffer=4, minRec=0.2, verbose=F) {
  playlist <- sample(songs) # initial shuffle
  len <- length(playlist)
  plays <- rep.int(0, len)
  if (verbose) {
    playback <- rep.int(0, n)
  }

  # decides how far back the song just played will be pushed
  # randomness should be kept low to avoid "repeats"
  # minRec sets a hard minimum on recycling zone
  # songs will never replay within the buffer
  recycle <- min(max(1, len - buffer), round(max(round(len * minRec), len * (1 - exp(-randomness * log(len))))))
  start <- max(1, len - recycle)

  # simulate playback
  for (i in 1:n) {
    if (len == 1) { # why are you shuffling
      plays[1] = plays[1] + 1
    }
    else {
      plays[playlist[1]] = plays[playlist[1]] + 1 # play next song
      if (verbose) {
        playback[i] = playlist[1]
      }
      shuffle <- round(runif(1, min=start, max=len)) + 0.5 # decide where song goes in order
      current <- (1:len)
      current[1] <- shuffle
      playlist <- playlist[order(current)] # reorder playlist
    }
  }
  if (!verbose) {
    return (plays)
  }
  return (list(plays=plays, playlist=playback))
}
