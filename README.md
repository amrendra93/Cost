Cost
====
getwd()
data = read.csv("SEIC.csv", header=T)

event <- read.csv("SEICevent.csv", colClass = c(rep("character", 5), "numeric", rep("character", 3)))


ev1<- event[,5]
ev1 <- strptime(ev1, format='%m/%d/%Y %H:%M')
event[,5] <- data.frame(ev1)
str(event[, 5])


##########################################


col1 <- data[,1:5]
data[, 1:5] <- apply(col1, 2, as.character)
str(data[,1:5])

col2 <- data[,6:23]
data[, 6:23] <- apply(col2, 2, as.numeric)
str(data[, 6:23])

col3 <- data[, 24:37]
data[, 24:37] <- apply(col3, 2, strptime, format='%m/%d/%Y %H:%M')
str(data[, 24:37])

col4 <- data[, c(38:42, 48:50, 53:56)]
data[, c(38:42, 48:50, 53:56)] <- apply(col4, 2, as.numeric)
str(data[, c(38:42, 48:50, 53:56)])

LOAD <- ifelse(data$FINALVOLUME > data$INITIALVOLUME, "Ballast", "Laden")
data$LOAD <- as.factor(LOAD)

########################################################################

# Time should be in “auto”, “secs”, “mins”, “hours”, “days”
x <- difftime(data$EOP, data$SOP, units = 'mins')
x <- difftime(data$EOP, data$SOP, units = 'hours')
x <- difftime(data$EOP, data$SOP, units = 'days')
x
l <- length(data$EOP)
data$FAOP[1:l-1] - data$SOP[2:l]
?ColClass

DISCHARGE <- with(data, INITIALVOLUME - FINALVOLUME)[data$LOAD == "Laden"]
VOL <- ifelse(data$LOAD=="Laden",data$INITIALVOLUME - data$FINALVOLUME, data$FINALVOLUME - data$INITIALVOLUME)
data$VOL <- VOL
mean(data$VOL[data$LOAD == "Laden"])
mean(data$VOL[data$LOAD == "Ballast"])


